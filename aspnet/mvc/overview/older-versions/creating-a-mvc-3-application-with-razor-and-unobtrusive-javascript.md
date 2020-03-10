---
uid: mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
title: Criando um aplicativo MVC 3 com Razor e JavaScript discreto | Microsoft Docs
author: microsoft
description: O aplicativo Web de exemplo user list demonstra como é simples criar aplicativos ASP.NET MVC 3 usando o mecanismo de exibição do Razor. O aplicativo de exemplo s...
ms.author: riande
ms.date: 11/01/2010
ms.assetid: 658b149b-d770-46bf-8b4b-4e47cca242f3
msc.legacyurl: /mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
msc.type: authoredcontent
ms.openlocfilehash: fb63493ff22c9261fc5746a998a32f2511141f87
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78540981"
---
# <a name="creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript"></a>Criação de um aplicativo do MVC 3 com Razor e JavaScript não obstrusivo

pela [Microsoft](https://github.com/microsoft)

> O aplicativo Web de exemplo user list demonstra como é simples criar aplicativos ASP.NET MVC 3 usando o mecanismo de exibição do Razor. O aplicativo de exemplo mostra como usar o novo mecanismo de exibição do Razor com o ASP.NET MVC versão 3 e o Visual Studio 2010 para criar um site da lista de usuários fictícios que inclui funcionalidades como criar, exibir, editar e excluir usuários.
> 
> Este tutorial descreve as etapas que foram tomadas para criar o aplicativo ASP.NET MVC 3 de exemplo de lista de usuários. Um projeto do Visual Studio C# com o e o código-fonte do vb está disponível para acompanhar este tópico: [Download](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114). Se você tiver dúvidas sobre este tutorial, poste-os no [Fórum do MVC](https://forums.asp.net/1146.aspx).

## <a name="overview"></a>Visão geral

O aplicativo que você criará é um site de lista de usuários simples. Os usuários podem inserir, exibir e atualizar as informações do usuário.

![Site de exemplo](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image1.png)

Você pode baixar o VB e C# o projeto concluído [aqui](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).

## <a name="creating-the-web-application"></a>Criando o aplicativo Web

Para iniciar o tutorial, abra o Visual Studio 2010 e crie um novo projeto usando o modelo de *aplicativo Web do ASP.NET MVC 3* . Nomeie o aplicativo &quot;Mvc3Razor&quot;.

[![novo projeto MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)

Na caixa de diálogo **novo projeto ASP.NET MVC 3** , selecione **aplicativo de Internet**, selecione o mecanismo de exibição Razor e clique em **OK**.

![Caixa de diálogo novo projeto ASP.NET MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image4.png)

Neste tutorial, você não usará o provedor de associação do ASP.NET, portanto, poderá excluir todos os arquivos associados ao logon e à associação. Em **Gerenciador de soluções**, remova os seguintes arquivos e diretórios:

- *Controllers\AccountController*
- *Models\AccountModels*
- *Views\Shared\\_LogOnPartial*
- *Views\Account* (e todos os arquivos neste diretório)

![Soln exp](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image5.png)

Edite o arquivo <em>layout. cshtml do\_</em> e substitua a marcação dentro do elemento `<div>` chamado `logindisplay` pela mensagem <em>&quot;</em>logon desabilitado&quot;. O exemplo a seguir mostra a nova marcação:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample1.cshtml)]

## <a name="adding-the-model"></a>Adicionando o modelo

Em **Gerenciador de soluções**, clique com o botão direito do mouse na pasta *modelos* , selecione **Adicionar**e clique em **classe**.

![Nova classe MDL de usuário](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image6.png)

Nome da classe `UserModel`. Substitua o conteúdo do arquivo *UserModel* pelo código a seguir:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample2.cs)]

A classe `UserModel` representa os usuários. Cada membro da classe é anotado com o atributo [Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) do namespace [Annotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) . Os atributos no namespace [Annotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) fornecem validação automática do cliente e do lado do servidor para aplicativos Web.

Abra a classe `HomeController` e adicione uma diretiva `using` para que você possa acessar as classes `UserModel` e `Users`:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample3.cs)]

Logo após a declaração de `HomeController`, adicione o comentário a seguir e a referência a uma classe `Users`:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample4.cs)]

A classe `Users` é um armazenamento de dados na memória simplificado que você usará neste tutorial. Em um aplicativo real, você usaria um banco de dados para armazenar informações do usuário. As primeiras linhas do arquivo de `HomeController` são mostradas no exemplo a seguir:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample5.cs)]

Compile o aplicativo de forma que o modelo de usuário estará disponível para o assistente de scaffolding na próxima etapa.

## <a name="creating-the-default-view"></a>Criando a exibição padrão

A próxima etapa é adicionar um método de ação e uma exibição para exibir os usuários.

Exclua o arquivo *Views\Home\Index* existente. Você criará um novo arquivo de *índice* para exibir os usuários.

Na classe `HomeController`, substitua o conteúdo do método `Index` pelo seguinte código:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample6.cs)]

Clique com o botão direito do mouse dentro do método `Index` e clique em **Adicionar exibição**.

![Adicionar modo de exibição](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image7.png)

Selecione a opção **criar uma exibição fortemente tipada** . Para **exibir a classe de dados**, selecione **Mvc3Razor. Models. UserModel**. (Se você não vir **Mvc3Razor. Models. UserModel** na caixa de **classe exibir dados** , você precisará compilar o projeto.) Verifique se o mecanismo de exibição está definido como **Razor**. Defina **Exibir conteúdo** como **lista** e clique em **Adicionar**.

![Adicionar exibição de índice](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image8.png)

A nova exibição aplica Scaffold automaticamente os dados do usuário que são passados para a exibição `Index`. Examine o arquivo *Views\Home\Index* gerado recentemente. Os links **criar novo**, **Editar**, **detalhes**e **excluir** não funcionam, mas o restante da página é funcional. Execute a página. Você verá uma lista de usuários.

![Página de índice](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image9.png)

Abra o arquivo *index. cshtml* e substitua a marcação de `ActionLink` para **Editar**, **detalhes**e **excluir** com o seguinte código:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample7.cshtml)]

O nome de usuário é usado como a ID para localizar o registro selecionado nos links **Editar**, **detalhes**e **excluir** .

## <a name="creating-the-details-view"></a>Criando a exibição de detalhes

A próxima etapa é adicionar um método de ação `Details` e exibir para exibir detalhes do usuário.

![Detalhes](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image10.png)

Adicione o seguinte método `Details` ao controlador Home:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample8.cs)]

Clique com o botão direito do mouse dentro do método `Details` e selecione <strong>Adicionar exibição</strong>. Verifique se a caixa de <strong>classe exibir dados</strong> contém <strong>Mvc3Razor. Models. UserModel</strong><em>.</em> Defina <strong>Exibir conteúdo</strong> como <strong>detalhes</strong> e clique em <strong>Adicionar</strong>.

![Adicionar exibição de detalhes](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image11.png)

Execute o aplicativo e selecione um link detalhes. O scaffolding automático mostra cada propriedade no modelo.

![Detalhes](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image12.png)

## <a name="creating-the-edit-view"></a>Criando o modo de exibição de edição

Adicione o seguinte método `Edit` ao controlador Home.

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample9.cs)]

Adicione uma exibição como nas etapas anteriores, mas defina **Exibir conteúdo** para **Editar**.

![Adicionar modo de exibição de edição](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image13.png)

Execute o aplicativo e edite o nome e o sobrenome de um dos usuários. Se você violar quaisquer `DataAnnotation` restrições que foram aplicadas à classe `UserModel`, ao enviar o formulário, você verá erros de validação que são produzidos pelo código do servidor. Por exemplo, se você alterar o nome &quot;Ana&quot; para &quot;um&quot;, quando enviar o formulário, o seguinte erro será exibido no formulário:

`The field First Name must be a string with a minimum length of 3 and a maximum length of 8.`

Neste tutorial, você está tratando o nome de usuário como a chave primária. Portanto, a propriedade nome de usuário não pode ser alterada. No arquivo *Edit. cshtml* , logo após a instrução `Html.BeginForm`, defina o nome de usuário como um campo oculto. Isso faz com que a propriedade seja passada no modelo. O fragmento de código a seguir mostra o posicionamento da instrução de `Hidden`:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample10.cshtml)]

Substitua o `TextBoxFor` e a marcação de `ValidationMessageFor` para o nome de usuário com uma chamada `DisplayFor`. O método `DisplayFor` exibe a propriedade como um elemento somente leitura. O exemplo a seguir mostra a marcação concluída. As chamadas originais `TextBoxFor` e `ValidationMessageFor` são comentadas com os caracteres de início de comentário do Razor e comentário final (`@* *@`)

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample11.cshtml)]

## <a name="enabling-client-side-validation"></a>Habilitando a validação do lado do cliente

Para habilitar a validação do lado do cliente no ASP.NET MVC 3, você deve definir dois sinalizadores e deve incluir três arquivos JavaScript.

Abra o arquivo *Web. config* do aplicativo. Verifique se `that ClientValidationEnabled` e `UnobtrusiveJavaScriptEnabled` estão definidos como true nas configurações do aplicativo. O fragmento a seguir do arquivo *Web. config* raiz mostra as configurações corretas:

[!code-xml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample12.xml)]

A definição de `UnobtrusiveJavaScriptEnabled` como True habilita a validação de cliente discreta e não discreta. Quando você usa a validação discreta, as regras de validação são transformadas em atributos HTML5. Os nomes de atributo HTML5 podem consistir apenas em letras minúsculas, números e traços.

A configuração de `ClientValidationEnabled` como True habilita a validação do lado do cliente. Ao definir essas chaves no arquivo *Web. config* do aplicativo, você habilita a validação do cliente e o JavaScript discreto para todo o aplicativo. Você também pode habilitar ou desabilitar essas configurações em modos de exibição individuais ou em métodos de controlador usando o seguinte código:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample13.cs)]

Você também precisa incluir vários arquivos JavaScript na exibição renderizada. Uma maneira fácil de incluir o JavaScript em todos os modos de exibição é adicioná-los ao arquivo *Views\Shared\\_Layout. cshtml* . Substitua o elemento `<head>` do arquivo *layout. cshtml de\_* pelo código a seguir:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample14.cshtml)]

Os dois primeiros scripts jQuery são hospedados pela CDN (rede de distribuição de conteúdo) do Microsoft Ajax. Aproveitando a CDN do Microsoft Ajax, você pode melhorar significativamente o desempenho inicial de seus aplicativos.

Execute o aplicativo e clique em um link de edição. Exiba a origem da página no navegador. A origem do navegador mostra muitos atributos do formulário `data-val` (para validação de dados). Quando a validação do cliente e o JavaScript discreto estão habilitados, os campos de entrada com uma regra de validação do cliente contêm o atributo `data-val="true"` para disparar a validação de cliente discreta. Por exemplo, o campo `City` no modelo foi decorado com o atributo [Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) , que resulta no HTML mostrado no exemplo a seguir:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample15.cshtml)]

Para cada regra de validação de cliente, é adicionado um atributo que tem o formulário `data-val-rulename="message"`. Usando o exemplo de campo `City` mostrado anteriormente, a regra de validação de cliente necessária gera o atributo `data-val-required` e a mensagem &quot;o campo cidade é necessário&quot;. Execute o aplicativo, edite um dos usuários e desmarque o campo `City`. Ao sair do campo, você verá uma mensagem de erro de validação no lado do cliente.

![Cidade necessária](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image14.png)

Da mesma forma, para cada parâmetro na regra de validação do cliente, é adicionado um atributo que tem o formulário `data-val-rulename-paramname=paramvalue`. Por exemplo, a propriedade `FirstName` é anotada com o atributo [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) e especifica um comprimento mínimo de 3 e um comprimento máximo de 8. A regra de validação de dados chamada `length` tem o nome do parâmetro `max` e o valor do parâmetro 8. O código a seguir mostra o HTML que é gerado para o campo `FirstName` quando você edita um dos usuários:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample16.cshtml)]

Para obter mais informações sobre a validação de cliente discreta, consulte a entrada [validação de cliente discreta no ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) no blog de Brad Wilson.

> [!NOTE]
> No ASP.NET MVC 3 beta, às vezes você precisa enviar o formulário para iniciar a validação do lado do cliente. Isso pode ser alterado para a versão final.

## <a name="creating-the-create-view"></a>Criando o modo de exibição de criação

A próxima etapa é adicionar um método de ação `Create` e exibir para permitir que o usuário crie um novo usuário. Adicione o seguinte método `Create` ao controlador Home:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample17.cs)]

Adicione uma exibição como nas etapas anteriores, mas defina **Exibir conteúdo** a ser **criado**.

![Create View](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image15.png)

Execute o aplicativo, selecione o link **criar** e adicione um novo usuário. O método `Create` aproveita automaticamente a validação do lado do cliente e do servidor. Tente inserir um nome de usuário que contenha espaço em branco, como &quot;Ben X&quot;. Quando você faz a Tabulação do campo nome de usuário, um erro de validação no lado do cliente (`White space is not allowed`) é exibido.

## <a name="add-the-delete-method"></a>Adicionar o método Delete

Para concluir o tutorial, adicione o seguinte método `Delete` ao controlador Home:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample18.cs)]

Adicione uma exibição de `Delete` como nas etapas anteriores, definindo o **conteúdo de exibição** a ser **excluído**.

![Excluir Exibição](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image16.png)

Agora você tem um aplicativo ASP.NET MVC 3 simples, mas totalmente funcional, com validação.
