---
uid: mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
title: Fornecer suporte à entrada do formulário de dados CRUD (criar, ler, atualizar, excluir) | Microsoft Docs
author: microsoft
description: A etapa 5 mostra como usar ainda mais a nossa classe DinnersController habilitando o suporte para edição, criação e exclusão de jantares com ele também.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: bbb976e5-6150-4283-a374-c22fbafe29f5
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
msc.type: authoredcontent
ms.openlocfilehash: b3123af9a1477bc496a0d229d628510fc202b6d2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78580552"
---
# <a name="provide-crud-create-read-update-delete-data-form-entry-support"></a>Fornecer suporte de CRUD (criar, ler, atualizar e excluir) ao formulário de entrada de dados

pela [Microsoft](https://github.com/microsoft)

[Baixar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Esta é a etapa 5 de um [tutorial de aplicativo "NerdDinner"](introducing-the-nerddinner-tutorial.md) gratuito que percorre como criar um aplicativo Web pequeno, mas completo usando o ASP.NET MVC 1.
> 
> A etapa 5 mostra como usar ainda mais a nossa classe DinnersController habilitando o suporte para edição, criação e exclusão de jantares com ele também.
> 
> Se você estiver usando o ASP.NET MVC 3, recomendamos seguir as [introdução com](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) os tutoriais da [loja de música](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) MVC 3 ou MVC.

## <a name="nerddinner-step-5-create-update-delete-form-scenarios"></a>Etapa 5: criar, atualizar, excluir cenários de formulário

Apresentamos os controladores e os modos de exibição e abordamos como usá-los para implementar uma experiência de listagem/detalhes para jantares no site. Nossa próxima etapa será levar ainda mais nossa classe DinnersController e habilitar o suporte para edição, criação e exclusão de jantares também.

### <a name="urls-handled-by-dinnerscontroller"></a>URLs manipuladas por DinnersController

Anteriormente adicionamos métodos de ação a DinnersController que implementou o suporte para duas URLs: */Dinners* e */Dinners/Details/[ID]* .

| **URL** | **VERBO** | **Finalidade** |
| --- | --- | --- |
| */Dinners/* | GET | Exibir uma lista HTML de futuros jantares. |
| */Dinners/Details/[ID]* | GET | Exibir detalhes sobre um jantar específico. |

Agora, adicionaremos métodos de ação para implementar três URLs adicionais: */Dinners/Edit/[ID]* , */Dinners/Create*e */Dinners/Delete/[ID]* . Essas URLs permitirão o suporte à edição de jantares existentes, à criação de novos jantares e à exclusão de jantares.

Suportaremos as interações de verbo HTTP GET e HTTP POST com essas novas URLs. As solicitações HTTP GET para essas URLs exibirão a exibição HTML inicial dos dados (um formulário preenchido com os dados do jantar no caso de "Edit", um formulário em branco no caso de "Create" e uma tela de confirmação de exclusão no caso de "Delete"). As solicitações HTTP POST para essas URLs salvarão/atualizarão os dados do jantar em nosso DinnerRepository (e daí no banco de dado).

| **URL** | **VERBO** | **Finalidade** |
| --- | --- | --- |
| */Dinners/Edit/[ID]* | GET | Exibe um formulário HTML editável populado com dados de jantar. |
| POST | Salve as alterações de formulário para um jantar específico no banco de dados. |
| */Dinners/Create* | GET | Exibe um formulário HTML vazio que permite aos usuários definir novos jantares. |
| POST | Crie um novo jantar e salve-o no banco de dados. |
| */Dinners/Delete/[ID]* | GET | Exibir tela de confirmação de exclusão. |
| POST | Exclui o jantar especificado do banco de dados. |

### <a name="edit-support"></a>Editar suporte

Vamos começar implementando o cenário de "edição".

#### <a name="the-http-get-edit-action-method"></a>O método de ação de edição HTTP-GET

Vamos começar implementando o comportamento de HTTP "GET" do nosso método Edit Action. Esse método será invocado quando a URL de */Dinners/Edit/[ID]* for solicitada. Nossa implementação terá a seguinte aparência:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample1.cs)]

O código acima usa o DinnerRepository para recuperar um objeto de jantar. Em seguida, ele renderiza um modelo de exibição usando o objeto de jantar. Como não passamos explicitamente um nome de modelo para o método auxiliar *View ()* , ele usará o caminho padrão baseado em Convenção para resolver o modelo de exibição:/views/Dinners/Edit.aspx.

Agora, vamos criar esse modelo de exibição. Faremos isso clicando com o botão direito do mouse no método Edit e selecionando o comando de menu de contexto "Add View":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image1.png)

Na caixa de diálogo "Adicionar exibição", indicaremos que estamos passando um objeto de jantar para nosso modelo de exibição como modelo e optamos por scaffoldr automaticamente um modelo de "edição":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image2.png)

Quando clicamos no botão "Add" (Adicionar), o Visual Studio adicionará um novo arquivo de modelo de exibição "Edit. aspx" para nós no diretório "\Views\Dinners". Ele também abrirá o novo modelo de exibição "Edit. aspx" no editor de código – preenchido com uma implementação inicial de Scaffold "Edit", como abaixo:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image3.png)

Vamos fazer algumas alterações no scaffold de "edição" padrão gerado e atualizar o modelo de exibição de edição para que o conteúdo seja mostrado abaixo (o que remove algumas das propriedades que não queremos expor):

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample2.aspx)]

Quando executarmos o aplicativo e solicitarmos a URL *"/Dinners/Edit/1"* , veremos a seguinte página:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image4.png)

A marcação HTML gerada por nossa exibição é parecida com a mostrada abaixo. É HTML padrão – com um &lt;formulário&gt; elemento que executa um HTTP POST para a URL */Dinners/Edit/1* quando o botão "salvar" &lt;entrada tipo = "enviar"/&gt; é enviado por push. Um HTML &lt;tipo de entrada = "texto"/&gt; elemento foi apresentado para cada propriedade editável:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image5.png)

#### <a name="htmlbeginform-and-htmltextbox-html-helper-methods"></a>Métodos auxiliares HTML. BeginForm () e HTML. TextBox ()

Nosso modelo de exibição "Edit. aspx" está usando vários métodos "HTML Helper": HTML. ValidationSummary (), HTML. BeginForm (), HTML. TextBox () e HTML. ValidationMessage (). Além de gerar marcação HTML para nós, esses métodos auxiliares fornecem tratamento de erro interno e suporte à validação.

##### <a name="htmlbeginform-helper-method"></a>Método auxiliar HTML. BeginForm ()

O método auxiliar HTML. BeginForm () é a saída do elemento HTML &lt;Form&gt; em nossa marcação. Em nosso modelo de exibição Edit. aspx, você observará que estamos aplicando uma C# instrução "using" ao usar esse método. A chave de abertura indica o início do &lt;formulário&gt; conteúdo, e a chave de fechamento é o que indica o final do elemento&gt; &lt;do formulário:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample3.cs)]

Como alternativa, se você achar que a abordagem de instrução "using" não é natural para um cenário como esse, você pode usar uma combinação de HTML. BeginForm () e HTML. endformat () (que faz a mesma coisa):

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample4.aspx)]

Chamar HTML. BeginForm () sem nenhum parâmetro fará com que ele gere um elemento de formulário que faça um HTTP-POST para a URL da solicitação atual. É por isso que nosso modo de exibição de edição gera uma *ação de&lt;formulário = "/Dinners/Edit/1" método = "post"&gt;* elemento. Poderíamos ter passado, como alternativa, parâmetros explícitos para HTML. BeginForm () se quiséssemos postar em uma URL diferente.

##### <a name="htmltextbox-helper-method"></a>Método auxiliar HTML. TextBox ()

Nossa exibição Edit. aspx usa o método auxiliar HTML. TextBox () para saída &lt;tipo de entrada = "texto"/elementos de&gt;:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample5.aspx)]

O método html. TextBox () acima usa um único parâmetro – que está sendo usado para especificar os atributos de ID/nome do &lt;tipo de entrada = "texto"/&gt; elemento para saída, bem como a propriedade do modelo para popular o valor da caixa de texto. Por exemplo, o objeto de jantar que passamos para o modo de exibição de edição tinha um valor de propriedade "title" de ".NET Futures" e, portanto, nosso método html. TextBox ("title") é chamado de saída: *&lt;ID de entrada = "title" Name = "título" Type = "text" value = ". net Futures"/&gt;* .

Como alternativa, podemos usar o primeiro parâmetro HTML. TextBox () para especificar a ID/nome do elemento e, em seguida, passar explicitamente o valor a ser usado como um segundo parâmetro:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample6.aspx)]

Muitas vezes, vamos querer executar a formatação personalizada no valor que é a saída. O método estático String. Format () interno do .NET é útil para esses cenários. Nosso modelo de exibição Edit. aspx está usando isso para formatar o valor EventDate (que é do tipo DateTime) para que ele não mostre segundos para o tempo:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample7.aspx)]

Um terceiro parâmetro para HTML. TextBox () pode, opcionalmente, ser usado para gerar atributos HTML adicionais. O trecho de código a seguir demonstra como renderizar um atributo size = "30" adicional e um atributo Class = "mycssclass" no &lt;tipo de entrada = "text"/&gt; elemento. Observe como podemos escapar o nome do atributo de classe usando uma "classe@" character because "" é uma palavra-chave reservada C#em:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample8.aspx)]

#### <a name="implementing-the-http-post-edit-action-method"></a>Implementando o método de ação HTTP-POST Edit

Agora temos a versão HTTP-GET do nosso método de ação de edição implementado. Quando um usuário solicita a URL */Dinners/Edit/1* , eles recebem uma página HTML semelhante à seguinte:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image6.png)

Pressionar o botão "salvar" faz com que um formulário poste na URL */Dinners/Edit/1* e envia o HTML &lt;entrada&gt; valores de formulário usando o verbo HTTP post. Agora, vamos implementar o comportamento HTTP POST do nosso método de ação de edição, que manipulará o salvamento do jantar.

Vamos começar adicionando um método de ação "Editar" sobrecarregado a nosso DinnersController que tem um atributo "AcceptVerbs" que indica que ele lida com cenários HTTP POST:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample9.cs)]

Quando o atributo [AcceptVerbs] é aplicado a métodos de ação sobrecarregados, o ASP.NET MVC manipula automaticamente as solicitações de expedição para o método de ação apropriado, dependendo do verbo HTTP de entrada. Solicitações HTTP POST para */Dinners/Edit/[ID]* URLs vão para o método Edit acima, enquanto todas as outras solicitações HTTP Verb para */Dinners/Edit/[ID]* URLs vão para o primeiro método de edição que implementamos (que não tinha um atributo `[AcceptVerbs]`).

| **Tópico lateral: por que diferenciar por verbos HTTP?** |
| --- |
| Você pode perguntar: por que estamos usando uma única URL e diferenciando seu comportamento por meio do verbo HTTP? Por que não ter apenas duas URLs separadas para lidar com o carregamento e salvar as alterações de edição? Por exemplo:/Dinners/Edit/[ID] para exibir o formulário inicial e/Dinners/Save/[ID] para lidar com a postagem de formulário para salvá-lo? A desvantagem de publicar duas URLs separadas é que nos casos em que publicamos em/Dinners/Save/2 e, em seguida, preciso exibir novamente o formulário HTML devido a um erro de entrada, o usuário final acabará tendo a URL/Dinners/Save/2 na barra de endereços do navegador (desde que essa era a URL em que o formulário foi Postado). Se o usuário final marcar essa página novamente para a lista de favoritos do navegador ou copiar/colar a URL e enviar por email para um amigo, eles acabarão salvando uma URL que não funcionará no futuro (já que essa URL depende dos valores de post). Ao expor uma única URL (como:/Dinners/Edit/[ID]) e diferenciar o processamento dela por verbo HTTP, é seguro para os usuários finais marcarem a página de edição e/ou enviarem a URL para outras pessoas. |

#### <a name="retrieving-form-post-values"></a>Recuperando valores de postagem de formulário

Há várias maneiras pelas quais podemos acessar parâmetros de formulário postados em nosso método HTTP POST "Edit". Uma abordagem simples é simplesmente usar a propriedade Request na classe base do controlador para acessar a coleção de formulários e recuperar os valores postados diretamente:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample10.cs)]

A abordagem acima é um pouco detalhada, mas, especialmente depois de adicionarmos a lógica de tratamento de erro.

Uma abordagem melhor para esse cenário é aproveitar o método auxiliar *UpdateModel ()* interno na classe base do controlador. Ele dá suporte à atualização das propriedades de um objeto que passamos usando os parâmetros de formulário de entrada. Ele usa a reflexão para determinar os nomes de propriedade no objeto e, em seguida, converte e atribui automaticamente valores a eles com base nos valores de entrada enviados pelo cliente.

Poderíamos usar o método UpdateModel () para simplificar nossa ação de edição HTTP-POST usando este código:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample11.cs)]

Agora podemos visitar a URL do */Dinners/Edit/1* e alterar o título do nosso jantar:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image7.png)

Quando clicamos no botão "salvar", executaremos uma postagem de formulário em nossa ação de edição e os valores atualizados serão persistidos no banco de dados. Em seguida, será redirecionado para a URL de detalhes do jantar (que exibirá os valores recentemente salvos):

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image8.png)

#### <a name="handling-edit-errors"></a>Tratamento de erros de edição

Nossa implementação HTTP-POST atual funciona bem, exceto quando há erros.

Quando um usuário faz um erro ao editar um formulário, precisamos garantir que o formulário seja exibido novamente com uma mensagem de erro informativa que os orienta para corrigi-lo. Isso inclui casos em que um usuário final posta uma entrada incorreta (por exemplo: uma cadeia de caracteres de data malformada), bem como casos em que o formato de entrada é válido, mas há uma violação de regra de negócio. Quando ocorrem erros, o formulário deve preservar os dados de entrada que o usuário inseriu originalmente para que eles não precisem reabastecer suas alterações manualmente. Esse processo deve ser repetido quantas vezes forem necessárias até que o formulário seja concluído com êxito.

O ASP.NET MVC inclui alguns recursos internos interessantes que facilitam o tratamento de erros e a reexibição de formulários. Para ver esses recursos em ação, vamos atualizar nosso método Edit Action com o seguinte código:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample12.cs)]

O código acima é semelhante à nossa implementação anterior, exceto que agora estamos encapsulando um bloco de tratamento de erros try/catch em nosso trabalho. Se ocorrer uma exceção ao chamar UpdateModel (), ou quando tentarmos salvar o DinnerRepository (que gerará uma exceção se o objeto de jantar que estamos tentando salvar for inválido devido a uma violação de regra em nosso modelo), nosso bloco de tratamento de erro catch será executados. Dentro dele, executamos um loop em todas as violações de regra existentes no objeto de jantar e as adicionamos a um objeto ModelState (que discutiremos em breve). Em seguida, reexibimos o modo de exibição.

Para ver isso funcionando, vamos executar novamente o aplicativo, editar um jantar e alterá-lo para ter um título vazio, um EventDate de "falso" e usar um número de telefone do Reino Unido com um valor de país de EUA. Quando pressionamos o botão "Save", nosso método HTTP POST Edit não poderá salvar o jantar (porque há erros) e exibirá novamente o formulário:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image9.png)

Nosso aplicativo tem uma experiência de erro razoável. Os elementos de texto com a entrada inválida são realçados em vermelho e as mensagens de erro de validação são exibidas para o usuário final sobre eles. O formulário também está preservando os dados de entrada que o usuário inseriu originalmente – para que eles não precisem reabastecer nada.

Como você pode perguntar, isso ocorreu? Como as caixas de texto title, EventDate e ContactPhone se realçam em vermelho e sabem para gerar os valores de usuário inseridos originalmente? E como as mensagens de erro são exibidas na lista na parte superior? A boa notícia é que isso não ocorreu pela mágica, em vez disso, porque usamos alguns dos recursos internos do MVC ASP.NET que facilitam a validação de entrada e os cenários de tratamento de erros.

#### <a name="understanding-modelstate-and-the-validation-html-helper-methods"></a>Entendendo ModelState e os métodos auxiliares HTML de validação

As classes de controlador têm uma coleção de propriedades "ModelState" que fornece uma maneira de indicar que há erros com um objeto de modelo que está sendo passado para uma exibição. As entradas de erro dentro da coleção ModelState identificam o nome da Propriedade do modelo com o problema (por exemplo: "title", "EventDate" ou "ContactPhone") e permitem que uma mensagem de erro amigável seja especificada (por exemplo: "título necessário").

O método auxiliar *UpdateModel ()* preenche automaticamente a coleção ModelState quando encontra erros ao tentar atribuir valores de formulário às propriedades no objeto de modelo. Por exemplo, a propriedade EventDate do nosso objeto jantar é do tipo DateTime. Quando o método UpdateModel () não pôde atribuir o valor de cadeia de caracteres "falso" a ele no cenário acima, o método UpdateModel () adicionou uma entrada à coleção ModelState indicando que ocorreu um erro de atribuição com essa propriedade.

Os desenvolvedores também podem escrever código para adicionar explicitamente entradas de erro à coleção ModelState, como estamos fazendo abaixo em nosso bloco de tratamento de erros "catch", que está populando a coleção ModelState com entradas com base nas violações de regra ativas no Objeto de jantar:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample13.cs)]

#### <a name="html-helper-integration-with-modelstate"></a>Integração do auxiliar HTML com ModelState

Métodos auxiliares HTML – como HTML. TextBox ()-Verifique a coleção ModelState ao renderizar a saída. Se houver um erro para o item, eles renderizarão o valor inserido pelo usuário e uma classe de erro CSS.

Por exemplo, em nossa exibição de "Editar", estamos usando o método auxiliar HTML. TextBox () para renderizar o EventDate do nosso objeto de jantar:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample14.aspx)]

Quando o modo de exibição foi renderizado no cenário de erro, o método html. TextBox () verificou a coleção ModelState para ver se houve erros associados à propriedade "EventDate" do nosso objeto de jantar. Quando determinado que houve um erro, ele gerou a entrada do usuário enviado ("falso") como o valor e adicionou uma classe de erro de CSS à &lt;tipo de entrada = "TextBox"/&gt; marcação gerada:

[!code-html[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample15.html)]

Você pode personalizar a aparência da classe de erro CSS para ter a aparência desejada. A classe de erro CSS padrão – "entrada-validação-erro" – é definida na folha de estilos *\content\site.css* e se parece com a seguinte:

[!code-css[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample16.css)]

Essa regra CSS é o que causou a realce de nossos elementos de entrada inválidos, como abaixo:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image10.png)

##### <a name="htmlvalidationmessage-helper-method"></a>Método auxiliar HTML. ValidationMessage ()

O método auxiliar HTML. ValidationMessage () pode ser usado para produzir a mensagem de erro ModelState associada a uma propriedade de modelo específica:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample17.aspx)]

O código acima gera: *&lt;span class = "Field-Validation-Error"&gt; o valor ' falso ' é inválido&lt;/span&gt;*

O método auxiliar HTML. ValidationMessage () também dá suporte a um segundo parâmetro que permite aos desenvolvedores substituir a mensagem de texto de erro exibida:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample18.aspx)]

O código acima gera: *&lt;span class = "Field-Validation-Error"&gt;\*&lt;/span&gt;* em vez do texto de erro padrão quando um erro está presente para a propriedade EventDate.

##### <a name="htmlvalidationsummary-helper-method"></a>Método auxiliar HTML. ValidationSummary ()

O método auxiliar HTML. ValidationSummary () pode ser usado para renderizar uma mensagem de erro de resumo, acompanhada por um &lt;UL&gt;&lt;li/&gt;&lt;/UL&gt; lista de todas as mensagens de erro detalhadas na coleção ModelState:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image11.png)

O método auxiliar HTML. ValidationSummary () usa um parâmetro de cadeia de caracteres opcional – que define uma mensagem de erro de resumo a ser exibida acima da lista de erros detalhados:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample19.aspx)]

Opcionalmente, você pode usar o CSS para substituir a aparência da lista de erros.

#### <a name="using-a-addruleviolations-helper-method"></a>Usando um método auxiliar AddRuleViolations

Nossa implementação inicial de edição HTTP-POST usou uma instrução Foreach dentro de seu bloco catch para executar um loop sobre as violações de regra do objeto de jantar e adicioná-las à coleção ModelState do controlador:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample20.cs)]

Podemos tornar esse código um pouco mais claro adicionando uma classe "ControllerHelpers" ao projeto NerdDinner e implementar um método de extensão "AddRuleViolations" dentro dele que adiciona um método auxiliar à classe ASP.NET MVC ModelStateDictionary. Esse método de extensão pode encapsular a lógica necessária para popular o ModelStateDictionary com uma lista de erros RuleViolation:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample21.cs)]

Em seguida, podemos atualizar nosso método de ação HTTP-POST Edit para usar esse método de extensão para popular a coleção ModelState com nossas violações de regra de jantar.

#### <a name="complete-edit-action-method-implementations"></a>Concluir as implementações do método de ação de edição

O código a seguir implementa toda a lógica do controlador necessária para nosso cenário de edição:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample22.cs)]

A boa coisa sobre nossa implementação de edição é que nem nossa classe de controlador nem nosso modelo de exibição precisa saber nada sobre a validação específica ou regras de negócio sendo impostas por nosso modelo de jantar. Podemos adicionar mais regras ao nosso modelo no futuro e não é necessário fazer nenhuma alteração de código em nosso controlador ou exibição para que eles tenham suporte. Isso nos fornece a flexibilidade para desenvolver facilmente nossos requisitos de aplicativo no futuro, com um mínimo de alterações de código.

### <a name="create-support"></a>Criar suporte

Acabamos de implementar o comportamento de "Editar" de nossa classe DinnersController. Agora, vamos passar para implementar o suporte "Create" (criar), que permitirá aos usuários adicionar novos jantares.

#### <a name="the-http-get-create-action-method"></a>O método de ação de criação HTTP-GET

Vamos começar implementando o comportamento de HTTP "GET" do nosso método Create Action. Esse método será chamado quando alguém visitar a URL do */Dinners/Create* . Nossa implementação é semelhante a:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample23.cs)]

O código acima cria um novo objeto de jantar e atribui sua propriedade EventDate a uma semana no futuro. Em seguida, ele renderiza uma exibição com base no novo objeto de jantar. Como não passamos explicitamente um nome para o método auxiliar *View ()* , ele usará o caminho padrão baseado em Convenção para resolver o modelo de exibição:/views/Dinners/Create.aspx.

Agora, vamos criar esse modelo de exibição. Podemos fazer isso clicando com o botão direito do mouse no método Create Action e selecionando o comando de menu de contexto "Add View". Na caixa de diálogo "Adicionar exibição", indicaremos que estamos passando um objeto de jantar para o modelo de exibição e optamos por scaffoldr automaticamente um modelo de "criação":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image12.png)

Quando clicamos no botão "Add" (Adicionar), o Visual Studio salvará uma nova exibição "Create. aspx" baseada em Scaffold no diretório "\Views\Dinners" e a abrirá no IDE:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image13.png)

Vamos fazer algumas alterações no arquivo de Scaffold "criar" padrão que foi gerado para nós e modificá-lo para que se pareça com o seguinte:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample24.aspx)]

E agora, quando executamos nosso aplicativo e acessamos a URL *"/Dinners/Create"* no navegador, ele renderizará a interface do usuário como a mostrada abaixo da nossa implementação de ação de criação:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image14.png)

#### <a name="implementing-the-http-post-create-action-method"></a>Implementando o método de ação HTTP-POST Create

Temos a versão HTTP-GET do nosso método Create Action implementado. Quando um usuário clica no botão "salvar", ele realiza uma postagem de formulário na URL */Dinners/Create* e envia o HTML &lt;entrada&gt; valores de formulário usando o verbo HTTP post.

Agora, vamos implementar o comportamento HTTP POST do nosso método Create Action. Vamos começar adicionando um método de ação sobrecarregado "Create" ao nosso DinnersController que tem um atributo "AcceptVerbs" que indica que ele lida com cenários HTTP POST:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample25.cs)]

Há várias maneiras pelas quais podemos acessar os parâmetros de formulário postados em nosso método "Create" habilitado para HTTP POST.

Uma abordagem é criar um novo objeto de jantar e, em seguida, usar o método auxiliar *UpdateModel ()* (como fizemos com a ação de edição) para preenchê-lo com os valores de formulário postados. Em seguida, podemos adicioná-lo ao nosso DinnerRepository, mantê-lo no banco de dados e redirecionar o usuário para nossa ação de detalhes para mostrar o jantar recém-criado usando o código abaixo:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample26.cs)]

Como alternativa, podemos usar uma abordagem em que temos nosso método de ação Create () pegar um objeto de jantar como um parâmetro de método. O ASP.NET MVC criará automaticamente um novo objeto de jantar para nós, preencherá suas propriedades usando as entradas do formulário e passará para o nosso método de ação:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample27.cs)]

Nosso método de ação acima verifica se o objeto de jantar foi preenchido com êxito com os valores de postagem do formulário, verificando a Propriedade ModelState. IsValid. Isso retornará false se houver problemas de conversão de entrada (por exemplo: uma cadeia de caracteres de "falso" para a propriedade EventDate) e, se houver algum problema, nosso método de ação reexibirá o formulário.

Se os valores de entrada forem válidos, o método de ação tentará adicionar e salvar o novo jantar no DinnerRepository. Ele encapsula esse trabalho em um bloco try/catch e exibe novamente o formulário se houver alguma violação de regra de negócio (o que faria com que o método dinnerRepository. Save () gere uma exceção).

Para ver esse comportamento de tratamento de erros em ação, podemos solicitar a URL do */Dinners/Create* e preencher os detalhes de um novo jantar. Entrada ou valores incorretos farão com que o formulário de criação seja exibido novamente com os erros realçados como abaixo:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image15.png)

Observe como nosso formulário de criação está respeitando exatamente as mesmas regras de validação e de negócios que o nosso formulário de edição. Isso ocorre porque nossa validação e regras de negócio foram definidas no modelo e não foram inseridas na interface do usuário nem no controlador do aplicativo. Isso significa que, posteriormente, podemos alterar/desenvolver nossas regras de validação ou de negócios em um único lugar e fazer com que elas se apliquem em todo o nosso aplicativo. Não precisaremos alterar nenhum código em nossos métodos de ação de edição ou de criação para atender automaticamente a quaisquer novas regras ou modificações nos existentes.

Quando corrigimos os valores de entrada e clicamos no botão "salvar" novamente, nossa adição ao DinnerRepository terá sucesso e um novo jantar será adicionado ao banco de dados. Em seguida, será redirecionado para a URL do */Dinners/Details/[ID]* – onde serão apresentados detalhes sobre o jantar recém-criado:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image16.png)

### <a name="delete-support"></a>Excluir suporte

Agora, vamos adicionar o suporte de "excluir" ao nosso DinnersController.

#### <a name="the-http-get-delete-action-method"></a>O método de ação de exclusão HTTP-GET

Começaremos implementando o comportamento HTTP GET do nosso método de ação de exclusão. Esse método será chamado quando alguém visitar a URL do */Dinners/Delete/[ID]* . Abaixo está a implementação:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample28.cs)]

O método de ação tenta recuperar o jantar a ser excluído. Se o jantar existir, ele renderiza uma exibição com base no objeto de jantar. Se o objeto não existir (ou já tiver sido excluído), ele retornará uma exibição que renderiza o modelo de exibição "não encontrado" criado anteriormente para nosso método de ação "detalhes".

Podemos criar o modelo de exibição "excluir" clicando com o botão direito do mouse no método de ação Excluir e selecionando o comando de menu de contexto "Adicionar exibição". Na caixa de diálogo "Adicionar exibição", indicaremos que estamos passando um objeto de jantar para nosso modelo de exibição como modelo e optamos por criar um modelo vazio:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image17.png)

Quando clicamos no botão "Add" (Adicionar), o Visual Studio adicionará um novo arquivo de modelo de exibição "Delete. aspx" para nós em nosso diretório "\Views\Dinners". Vamos adicionar um HTML e código ao modelo para implementar uma tela de confirmação de exclusão, como abaixo:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample29.aspx)]

O código acima exibe o título do jantar a ser excluído e gera um &lt;formulário&gt; elemento que faz um POST para a URL do/Dinners/Delete/[ID] se o usuário final clicar no botão "excluir" dentro dele.

Quando executamos nosso aplicativo e acessamos a URL *"/Dinners/Delete/[ID]"* para um objeto de jantar válido, ele renderiza a interface do usuário da seguinte maneira:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image18.png)

| **Tópico lateral: por que estamos fazendo uma POSTAgem?** |
| --- |
| Você pode perguntar: por que passamos pelo esforço de criar um formulário de &lt;&gt; em nossa tela de confirmação de exclusão? Por que não usar apenas um hiperlink padrão para vincular a um método de ação que faz a operação de exclusão real? O motivo é que queremos ter cuidado para se proteger contra rastreadores da Web e mecanismos de pesquisa descobrindo nossas URLs e fazendo com que os dados sejam excluídos inadvertidamente ao seguirem os links. As URLs baseadas em HTTP GET são consideradas "seguras" para que sejam acessadas/rastreadas, e elas devem não seguir as POSTAgens HTTP. Uma boa regra é garantir que você sempre coloque as operações destrutivas ou de modificação de dados por trás das solicitações HTTP-POST. |

#### <a name="implementing-the-http-post-delete-action-method"></a>Implementando o método de ação HTTP-POST Delete

Agora temos a versão HTTP-GET do nosso método de ação de exclusão implementado, que exibe uma tela de confirmação de exclusão. Quando um usuário final clica no botão "excluir", ele executará uma postagem de formulário na URL de */Dinners/Dinner/[ID]* .

Agora, vamos implementar o comportamento de HTTP "POST" do método de ação de exclusão usando o código abaixo:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample30.cs)]

A versão HTTP-POST do nosso método de ação de exclusão tenta recuperar o objeto de jantar a ser excluído. Se ele não conseguir encontrá-lo (porque ele já foi excluído), ele renderiza o modelo "não encontrado". Se encontrar o jantar, ele o excluirá do DinnerRepository. Em seguida, ele renderiza um modelo "excluído".

Para implementar o modelo "excluído", clicaremos com o botão direito do mouse no método de ação e escolheremos o menu de contexto "Adicionar exibição". Vamos nomear nossa exibição "Deleted" e ser um modelo vazio (e não pegar um objeto de modelo fortemente tipado). Em seguida, adicionaremos um conteúdo HTML a ele:

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample31.aspx)]

E agora, quando executamos nosso aplicativo e acessamos a URL *"/Dinners/Delete/[ID]"* para um objeto de jantar válido, ele renderizará nossa tela de confirmação de exclusão do jantar, como abaixo:

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image19.png)

Quando clicamos no botão "excluir", ele executará um HTTP-POST para a URL de */Dinners/Delete/[ID]* , que excluirá o jantar do nosso banco de dados e exibirá nosso modelo de exibição "excluído":

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image20.png)

### <a name="model-binding-security"></a>Segurança de associação de modelo

Discutimos duas maneiras diferentes de usar os recursos internos de associação de modelo do ASP.NET MVC. O primeiro é usar o método UpdateModel () para atualizar propriedades em um objeto de modelo existente e o segundo usando o suporte do ASP.NET MVC para passar objetos de modelo no como parâmetros do método de ação. Essas duas técnicas são muito poderosas e extremamente úteis.

Essa energia também traz a responsabilidade de ti. É importante sempre ser paranóicos sobre segurança ao aceitar qualquer entrada do usuário, e isso também é verdadeiro ao associar objetos à entrada de formulário. Você deve ter cuidado para sempre codificar em HTML qualquer valor inserido pelo usuário para evitar ataques de injeção de HTML e JavaScript e ter cuidado com ataques de injeção de SQL (Observação: estamos usando LINQ to SQL para nosso aplicativo, que codifica automaticamente os parâmetros para impedir que eles tipos de ataques). Você nunca deve confiar apenas na validação do lado do cliente e sempre empregar a validação do lado do servidor para se proteger contra hackers tentando enviar valores falsos.

Um item de segurança adicional para certificar-se de que você considere ao usar os recursos de associação do ASP.NET MVC é o escopo dos objetos que você está associando. Especificamente, você deseja ter certeza de que entendeu as implicações de segurança das propriedades que você está permitindo que estejam associadas e garantir que só seja possível atualizá-las por um usuário final para ser atualizada.

Por padrão, o método UpdateModel () tentará atualizar todas as propriedades no objeto de modelo que correspondem aos valores de parâmetro de formulário de entrada. Da mesma forma, os objetos passados como parâmetros de método de ação também podem ter todas as suas propriedades definidas por meio de parâmetros de formulário.

#### <a name="locking-down-binding-on-a-per-usage-basis"></a>Bloqueando a associação em uma base por uso

Você pode bloquear a política de associação por uso fornecendo uma "lista de inclusões" explícita de propriedades que podem ser atualizadas. Isso pode ser feito passando um parâmetro de matriz de cadeia de caracteres extra para o método UpdateModel (), como abaixo:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample32.cs)]

Objetos passados como parâmetros de método de ação também dão suporte a um atributo [BIND] que permite que uma "lista de inclusões" de propriedades permitidas seja especificada como abaixo:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample33.cs)]

#### <a name="locking-down-binding-on-a-type-basis"></a>Bloqueando a associação com base em um tipo

Você também pode bloquear as regras de associação de acordo com o tipo. Isso permite que você especifique as regras de associação uma vez e, em seguida, elas se aplicam em todos os cenários (incluindo cenários de parâmetro de método de ação e UpdateModel) em todos os controladores e métodos de ação.

Você pode personalizar as regras de associação por tipo adicionando um atributo [BIND] em um tipo ou registrando-o no arquivo global. asax do aplicativo (útil para cenários em que você não possui o tipo). Você pode usar as propriedades include e Exclude do atributo BIND para controlar quais propriedades são vinculáveis para a classe ou interface específica.

Usaremos essa técnica para a classe de jantar em nosso aplicativo NerdDinner e adicionaremos um atributo [BIND] a ela que restringe a lista de propriedades vinculáveis ao seguinte:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample34.cs)]

Observe que não estamos permitindo que a coleção de RSVPs seja manipulada por meio de associação, nem estamos permitindo que as propriedades Jantarid ou HostedBy sejam definidas por meio de associação. Por motivos de segurança, em vez disso, só manipularemos essas propriedades específicas usando código explícito dentro de nossos métodos de ação.

### <a name="crud-wrap-up"></a>Encapsulamento CRUD

O ASP.NET MVC inclui vários recursos internos que ajudam na implementação de cenários de lançamento de formulário. Usamos uma variedade desses recursos para fornecer suporte à interface do usuário CRUD sobre nosso DinnerRepository.

Estamos usando uma abordagem com foco no modelo para implementar nosso aplicativo. Isso significa que toda a lógica de validação e regra de negócio é definida em nossa camada de modelo – e não em nossos controladores ou exibições. Nem nossa classe de controlador nem nossos modelos de exibição sabem nada sobre as regras de negócios específicas sendo impostas por nossa classe de modelo de jantar.

Isso manterá nossa arquitetura de aplicativo limpa e facilitará o teste. Podemos adicionar mais regras de negócios à nossa camada de modelo no futuro e *não é necessário fazer nenhuma alteração de código* em nosso controlador ou exibição para que eles tenham suporte. Isso vai nos fornecer uma grande agilidade para evoluir e alterar nosso aplicativo no futuro.

Nosso DinnersController agora habilita listagens/detalhes de jantar, bem como suporte para criar, editar e excluir. O código completo para a classe pode ser encontrado abaixo:

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample35.cs)]

### <a name="next-step"></a>Próxima etapa

Agora temos a implementação básica de suporte CRUD (criar, ler, atualizar e excluir) em nossa classe DinnersController.

Agora, vamos examinar como podemos usar as classes ViewData e ViewModel para habilitar uma interface do usuário ainda mais rica em nossos formulários.

> [!div class="step-by-step"]
> [Anterior](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
> [Próximo](use-viewdata-and-implement-viewmodel-classes.md)
