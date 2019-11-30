---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: Habilitando operações CRUD no ASP.NET Web API 1-ASP.NET 4. x
author: MikeWasson
description: O tutorial mostra como dar suporte a operações CRUD em um serviço HTTP usando ASP.NET Web API para ASP.NET 4. x.
ms.author: riande
ms.date: 01/28/2012
ms.custom: seoapril2019
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: a096fd1c54df33b40115907a5c2517b2e3fec5b8
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600331"
---
# <a name="enabling-crud-operations-in-aspnet-web-api-1"></a>Habilitando operações CRUD no ASP.NET Web API 1

por [Mike Wasson](https://github.com/MikeWasson)

[Baixar projeto concluído](https://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> Este tutorial mostra como dar suporte a operações CRUD em um serviço HTTP usando ASP.NET Web API para ASP.NET 4. x.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Visual Studio 2012
> - API Web 1 (também funciona com a API Web 2)

CRUD significa &quot;criar, ler, atualizar e excluir&quot; que são as quatro operações básicas de banco de dados. Muitos serviços HTTP também modelam as operações CRUD por meio de APIs do tipo REST ou REST.

Neste tutorial, você criará uma API Web muito simples para gerenciar uma lista de produtos. Cada produto conterá um nome, preço e categoria (como &quot;Toys&quot; ou &quot;&quot;de hardware), além de uma ID de produto.

A API de produtos irá expor os métodos a seguir.

| Action | Método HTTP | URI relativo |
| --- | --- | --- |
| Obter uma lista de todos os produtos | Obter | /api/products |
| Obter um produto por ID | Obter | *ID* do/API/Products/ |
| Obter um produto por categoria | Obter | /API/Products? categoria =*categoria* |
| Criar um novo produto | Postar | /api/products |
| Atualizar um produto | PUT | *ID* do/API/Products/ |
| Excluir um produto | DELETE | *ID* do/API/Products/ |

Observe que alguns dos URIs incluem a ID do produto no caminho. Por exemplo, para obter o produto cuja ID é 28, o cliente envia uma solicitação GET para `http://hostname/api/products/28`.

### <a name="resources"></a>Recursos

A API Products define URIs para dois tipos de recursos:

| Resource | {1&gt;URI&lt;1} |
| --- | --- |
| A lista de todos os produtos. | /api/products |
| Um produto individual. | *ID* do/API/Products/ |

### <a name="methods"></a>{1&gt;Métodos&lt;1}

Os quatro principais métodos HTTP (GET, PUT, POST e DELETE) podem ser mapeados para operações CRUD da seguinte maneira:

- GET recupera a representação do recurso em um URI especificado. GET não deve ter efeitos colaterais no servidor.
- PUT atualiza um recurso em um URI especificado. PUT também pode ser usado para criar um novo recurso em um URI especificado, se o servidor permitir que os clientes especifiquem novos URIs. Para este tutorial, a API não dará suporte à criação por meio de PUT.
- POST cria um novo recurso. O servidor atribui o URI para o novo objeto e retorna esse URI como parte da mensagem de resposta.
- Excluir exclui um recurso em um URI especificado.

Observação: o método PUT substitui toda a entidade Product. Ou seja, espera-se que o cliente envie uma representação completa do produto atualizado. Se você quiser dar suporte a atualizações parciais, o método PATCH é preferencial. Este tutorial não implementa PATCH.

## <a name="create-a-new-web-api-project"></a>Criar um novo projeto de API Web

Comece executando o Visual Studio e selecione **novo projeto** na página **inicial** . Ou, no menu **arquivo** , selecione **novo** e **projeto**.

No painel **modelos** , selecione **modelos instalados** e expanda o **nó C# Visual** . Em **Visual C#** , selecione **Web**. Na lista de modelos de projeto, selecione **aplicativo Web ASP.NET MVC 4**. Nomeie o projeto &quot;ProductStore&quot; e clique em **OK**.

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

Na caixa de diálogo **novo projeto ASP.NET MVC 4** , selecione **API Web** e clique em **OK**.

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a>Adicionar um modelo

Um *modelo* é um objeto que representa os dados em seu aplicativo. No ASP.NET Web API, você pode usar objetos CLR com rigidez de tipos como modelos e eles serão automaticamente serializados para XML ou JSON para o cliente.

Para a API ProductStore, nossos dados consistem em produtos, então vamos criar uma nova classe chamada `Product`.

Se Gerenciador de Soluções ainda não estiver visível, clique no menu **Exibir** e selecione **Gerenciador de soluções**. Em Gerenciador de Soluções, clique com o botão direito do mouse na pasta **modelos** . No menu de contexto, selecione **Adicionar**e, em seguida, selecione **classe**. Nomeie a classe &quot;produto&quot;.

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

Adicione as propriedades a seguir à classe `Product`.

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a>Adicionando um repositório

Precisamos armazenar uma coleção de produtos. É uma boa ideia separar a coleção de nossa implementação de serviço. Dessa forma, podemos alterar o armazenamento de backup sem reescrever a classe de serviço. Esse tipo de design é chamado de padrão de *repositório* . Comece definindo uma interface genérica para o repositório.

Em Gerenciador de Soluções, clique com o botão direito do mouse na pasta **modelos** . Selecione **Adicionar**e, em seguida, selecione **novo item**.

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

No painel **modelos** , selecione **modelos instalados** e expanda o C# nó. Em C#, selecione **código**. Na lista de modelos de código, selecione **interface**. Nomeie a interface &quot;IProductRepository&quot;.

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

Adicione a seguinte implementação:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

Agora, adicione outra classe à pasta modelos, chamada &quot;ProductRepository.&quot; essa classe implementará a interface `IProductRepository`. Adicione a seguinte implementação:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

O repositório mantém a lista na memória local. Isso é certo para um tutorial, mas em um aplicativo real, você armazenaria os dados externamente, um banco de dado ou em armazenamento em nuvem. O padrão de repositório tornará mais fácil alterar a implementação posteriormente.

## <a name="adding-a-web-api-controller"></a>Adicionando um controlador de API Web

Se você trabalhou com o ASP.NET MVC, já está familiarizado com os controladores. No ASP.NET Web API, um *controlador* é uma classe que MANIPULA solicitações HTTP do cliente. O assistente de novo projeto criou dois controladores para você quando ele criou o projeto. Para vê-los, expanda a pasta controladores em Gerenciador de Soluções.

- HomeController é um controlador MVC ASP.NET tradicional. Ele é responsável por servir páginas HTML para o site e não está diretamente relacionado à nossa API Web.
- ValuesController é um controlador WebAPI de exemplo.

Vá em frente e exclua ValuesController clicando com o botão direito do mouse no arquivo em Gerenciador de Soluções e selecionando **excluir.** Agora, adicione um novo controlador, da seguinte maneira:

Em **Gerenciador de soluções**, clique com o botão direito do mouse na pasta controladores. Selecione **Adicionar** e, em seguida, selecione **controlador**.

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

No Assistente para **Adicionar controlador** , nomeie o controlador &quot;ProductsController&quot;. Na lista suspensa **modelo** , selecione **controlador de API vazio**. Em seguida, clique em **Adicionar**.

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> Não é necessário colocar os controladores em uma pasta chamada Controllers. O nome da pasta não é importante; é simplesmente uma maneira conveniente de organizar os arquivos de origem.

O assistente para **Adicionar controlador** criará um arquivo chamado ProductsController.cs na pasta controladores. Se esse arquivo ainda não estiver aberto, clique duas vezes no arquivo para abri-lo. Adicione a seguinte instrução **using** :

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

Adicione um campo que contém uma instância de **IProductRepository** .

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> Chamar `new ProductRepository()` no controlador não é o melhor design, pois ele vincula o controlador a uma implementação específica de `IProductRepository`. Para obter uma abordagem melhor, consulte [usando o resolvedor de dependência da API Web](../advanced/dependency-injection.md).

## <a name="getting-a-resource"></a>Obtendo um recurso

A API ProductStore irá expor várias ações de&quot; de leitura &quot;como métodos GET HTTP. Cada ação corresponderá a um método na classe `ProductsController`.

| Action | Método HTTP | URI relativo |
| --- | --- | --- |
| Obter uma lista de todos os produtos | Obter | /api/products |
| Obter um produto por ID | Obter | *ID* do/API/Products/ |
| Obter um produto por categoria | Obter | /API/Products? categoria =*categoria* |

Para obter a lista de todos os produtos, adicione este método à classe `ProductsController`:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

O nome do método começa com &quot;obter&quot;, portanto, por convenção, ele é mapeado para solicitações GET. Além disso, como o método não tem parâmetros, ele é mapeado para um URI que não contém uma *ID de&quot;&quot;* segmento no caminho.

Para obter um produto por ID, adicione este método à classe `ProductsController`:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

Esse nome de método também começa com &quot;obter&quot;, mas o método tem um parâmetro chamado *ID*. Esse parâmetro é mapeado para a ID de &quot;&quot; segmento do caminho do URI. A estrutura de ASP.NET Web API converte automaticamente a ID para o tipo de dados correto (**int**) para o parâmetro.

O método getproduct gera uma exceção do tipo **httpresponseexception** se a *ID* não for válida. Essa exceção será convertida pelo Framework em um erro 404 (não encontrado).

Por fim, adicione um método para localizar produtos por categoria:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

Se o URI de solicitação tiver uma cadeia de caracteres de consulta, a API Web tentará corresponder os parâmetros de consulta aos parâmetros no método do controlador. Portanto, um URI do formato "API/produtos? Category =*Category*" será mapeado para esse método.

## <a name="creating-a-resource"></a>Criando um recurso

Em seguida, adicionaremos um método à classe `ProductsController` para criar um novo produto. Aqui está uma implementação simples do método:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

Observe duas coisas sobre este método:

- O nome do método começa com &quot;post...&quot;. Para criar um novo produto, o cliente envia uma solicitação HTTP POST.
- O método usa um parâmetro do tipo Product. Na API Web, os parâmetros com tipos complexos são desserializados do corpo da solicitação. Portanto, esperamos que o cliente envie uma representação serializada de um objeto Product, no formato XML ou JSON.

Essa implementação funcionará, mas não está completamente completa. Idealmente, gostaríamos que a resposta HTTP incluísse o seguinte:

- **Código de resposta:** Por padrão, a estrutura da API da Web define o código de status de resposta como 200 (OK). Mas, de acordo com o protocolo HTTP/1.1, quando uma solicitação POST resulta na criação de um recurso, o servidor deve responder com o status 201 (criado).
- **Local:** Quando o servidor cria um recurso, ele deve incluir o URI do novo recurso no cabeçalho de local da resposta.

ASP.NET Web API torna mais fácil manipular a mensagem de resposta HTTP. Esta é a implementação aprimorada:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

Observe que o tipo de retorno do método agora é **HttpResponseMessage**. Ao retornar um **HttpResponseMessage** em vez de um produto, podemos controlar os detalhes da mensagem de resposta http, incluindo o código de status e o cabeçalho do local.

O método **CreateResponse** cria um **HttpResponseMessage** e grava automaticamente uma representação serializada do objeto Product no corpo da mensagem de resposta.

> [!NOTE]
> Este exemplo não valida o `Product`. Para obter informações sobre a validação de modelo, consulte [validação de modelo em ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).

## <a name="updating-a-resource"></a>Atualizando um recurso

A atualização de um produto com PUT é simples:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

O nome do método começa com &quot;put...&quot;, portanto, a API Web faz a correspondência para colocar solicitações. O método usa dois parâmetros, a ID do produto e o produto atualizado. O parâmetro *ID* é extraído do caminho do URI e o parâmetro *Product* é desserializado do corpo da solicitação. Por padrão, a estrutura de ASP.NET Web API usa tipos de parâmetro simples da rota e dos tipos complexos do corpo da solicitação.

## <a name="deleting-a-resource"></a>Excluindo um recurso

Para excluir um recurso, defina um "excluir..." forma.

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

Se uma solicitação de exclusão for realizada com sucesso, ela poderá retornar o status 200 (OK) com um corpo de entidade que descreve o status; status 202 (aceito) se a exclusão ainda estiver pendente; ou o status 204 (sem conteúdo) sem corpo de entidade. Nesse caso, o método `DeleteProduct` tem um tipo de retorno `void`, portanto ASP.NET Web API converte automaticamente isso no código de status 204 (sem conteúdo).
