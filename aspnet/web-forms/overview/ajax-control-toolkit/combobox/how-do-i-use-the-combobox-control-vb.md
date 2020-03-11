---
uid: web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-vb
title: Como fazer usar o controle ComboBox? (VB) | Microsoft Docs
author: microsoft
description: ComboBox é um controle ASP.NET AJAX que combina a flexibilidade de uma TextBox com uma lista de opções das quais os usuários podem escolher.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: e887e7b2-a6e7-4a28-a134-ba334494badb
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 468063a72253cce55a02bfaef1219bff03d06418
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78554484"
---
# <a name="how-do-i-use-the-combobox-control-vb"></a>Como fazer usar o controle ComboBox? (VB)

pela [Microsoft](https://github.com/microsoft)

> ComboBox é um controle ASP.NET AJAX que combina a flexibilidade de uma TextBox com uma lista de opções das quais os usuários podem escolher.

O objetivo deste tutorial é explicar o controle ComboBox do AJAX Control Toolkit. A ComboBox funciona como uma combinação entre um controle ASP.NET DropDownList padrão e um controle TextBox. Você pode selecionar uma lista de itens pré-existente ou inserir um novo item.

A ComboBox é semelhante ao extensor de controle de preenchimento automático, mas os controles são usados em cenários diferentes. O extensor de preenchimento automático consulta um serviço Web para obter entradas correspondentes. O controle ComboBox, por sua vez, é inicializado com um conjunto de itens. Usar o extensor de preenchimento automático faz sentido quando você está trabalhando com um grande conjunto de dados (milhões de partes de carro) ao usar o controle ComboBox faz sentido ao trabalhar com um pequeno conjunto de dados (dezenas de peças de carro).

## <a name="selecting-from-a-static-list-of-items"></a>Selecionando em uma lista estática de itens

Vamos começar com um exemplo simples de como usar o controle ComboBox. Imagine que você deseja exibir uma lista estática de itens em uma lista suspensa. No entanto, você deseja deixar de abrir a possibilidade de que a lista não seja concluída. Você deseja permitir que um usuário insira um valor personalizado na lista.

Criamos uma nova página de Web Forms ASP.NET e usamos o controle ComboBox na página. Adicione a nova página ASP.NET ao seu projeto e alterne para modo de exibição de Design.

Se você quiser usar o controle ComboBox na página, deverá adicionar um controle ScriptManager à página. Arraste o controle ScriptManager sob a guia extensões AJAX até a superfície do designer. Você deve adicionar o controle ScriptManager na parte superior da página; Você pode adicioná-lo imediatamente abaixo do formulário de &lt;de abertura do lado do servidor&gt; marca.

Em seguida, arraste o controle ComboBox para a página. Você pode encontrar o controle ComboBox na caixa de ferramentas com os outros controles do AJAX Control Toolkit e extensores de controle (consulte Figura 1).

[![formulário simples para criar um cartão de visita](how-do-i-use-the-combobox-control-vb/_static/image1.jpg)](how-do-i-use-the-combobox-control-vb/_static/image1.png)

**Figura 01**: selecionando o controle ComboBox na caixa de ferramentas ([clique para exibir a imagem em tamanho normal](how-do-i-use-the-combobox-control-vb/_static/image2.png))

Usaremos o controle ComboBox para exibir uma lista estática de opções. O usuário pode selecionar um nível específico de spiciness para sua comida de uma lista de três opções: leve, médio e quente (consulte a Figura 2).

[![selecionando em uma lista estática de itens](how-do-i-use-the-combobox-control-vb/_static/image2.jpg)](how-do-i-use-the-combobox-control-vb/_static/image3.png)

**Figura 02**: selecionando em uma lista estática de itens ([clique para exibir a imagem em tamanho normal](how-do-i-use-the-combobox-control-vb/_static/image4.png))

Há duas maneiras pelas quais você pode adicionar essas opções ao controle ComboBox. Primeiro, você seleciona a opção de tarefa editar opções ao posicionar o mouse sobre o controle em modo de exibição de Design e abrir o editor de item (consulte a Figura 3).

[![editar itens ComboBox](how-do-i-use-the-combobox-control-vb/_static/image3.jpg)](how-do-i-use-the-combobox-control-vb/_static/image5.png)

**Figura 03**: editando itens de ComboBox ([clique para exibir a imagem em tamanho normal](how-do-i-use-the-combobox-control-vb/_static/image6.png))

A segunda opção é adicionar a lista de itens entre a abertura e o fechamento &lt;ASP: ComboBox&gt; marcas na exibição de código-fonte. A página na Listagem 1 contém a ComboBox atualizada que tem a lista de itens.

**Listagem 1-static. aspx**

[!code-aspx[Main](how-do-i-use-the-combobox-control-vb/samples/sample1.aspx)]

Ao abrir a página na Listagem 1, você pode selecionar uma das opções preexistentes da ComboBox. Em outras palavras, a ComboBox funciona exatamente como um controle DropDownList.

No entanto, você também tem a opção de inserir uma nova opção (por exemplo, super Spicy) que não está na lista existente. Portanto, a ComboBox também funciona como um controle TextBox.

Independentemente de você escolher um item pré-existente ou inserir um item personalizado, ao enviar o formulário, sua escolha aparecerá no controle rótulo. Quando você envia o formulário, o manipulador btnSubmit\_clique executa e atualiza o rótulo (consulte a Figura 4).

[![exibir o item selecionado](how-do-i-use-the-combobox-control-vb/_static/image4.jpg)](how-do-i-use-the-combobox-control-vb/_static/image7.png)

**Figura 04**: exibindo o item selecionado ([clique para exibir a imagem em tamanho normal](how-do-i-use-the-combobox-control-vb/_static/image8.png))

A ComboBox oferece suporte às mesmas propriedades que o controle DropDownList para recuperar o item selecionado depois que um formulário é enviado:

- SelectedItem. Text – exibe o valor da propriedade Text do item selecionado.
- SelectedItem. Value-exibe o valor da propriedade Value do item selecionado ou exibe o texto digitado na ComboBox.
- SelectedValue-igual a SelectedItem. Value, exceto que essa propriedade permite que você especifique o item selecionado (inicial) padrão.

Se você digitar uma opção personalizada na ComboBox, a escolha personalizada será atribuída às propriedades SelectedItem. Text e SelectedItem. Value.

## <a name="selecting-the-list-of-items-from-the-database"></a>Selecionando a lista de itens do banco de dados

Você pode recuperar a lista de itens que a ComboBox exibe de um banco de dados. Por exemplo, você pode associar a ComboBox a um controle SqlDataSource, um controle ObjectDataSource, um LinqDataSource ou uma EntityDataSource.

Imagine que você deseja exibir uma lista de filmes em uma ComboBox. Você deseja recuperar a lista de filmes da tabela de banco de dados de filmes. Siga estas etapas:

1. Crie uma página chamada Movies. aspx
2. Adicione um controle ScriptManager à página arrastando o ScriptManager de sob a guia extensões AJAX na caixa de ferramentas para a página.
3. Adicione um controle ComboBox à página arrastando a ComboBox para a página.
4. Em modo de exibição de Design, passe o mouse sobre o controle ComboBox e selecione a opção de tarefa **escolher fonte de dados** (veja a Figura 5). O assistente de configuração da fonte de dados é iniciado.
5. Na etapa **escolher uma fonte de dados** , selecione a opção &lt;nova fonte de dados&gt;.
6. Na etapa **escolher um tipo de fonte de dados** , selecione Database.
7. Na etapa **escolher sua conexão de dados** , selecione seu banco de dado (por exemplo, MoviesDB. MDF).
8. Na etapa **salvar a cadeia de conexão no arquivo de configuração do aplicativo** , selecione a opção para salvar a cadeia de conexão.
9. Na etapa **Configurar a instrução SELECT** , selecione a tabela de banco de dados filmes e selecione todas as colunas.
10. Na etapa **testar consulta** , clique no botão Concluir.
11. De volta à etapa **escolher fonte de dados** , selecione a coluna título para o campo a ser exibido e a coluna ID para o campo de dados (consulte a figura).
12. Clique no botão OK para fechar o assistente.

[![escolher uma fonte de dados](how-do-i-use-the-combobox-control-vb/_static/image5.jpg)](how-do-i-use-the-combobox-control-vb/_static/image9.png)

**Figura 05**: escolhendo uma fonte de dados ([clique para exibir a imagem em tamanho normal](how-do-i-use-the-combobox-control-vb/_static/image10.png))

[![escolhendo os campos de texto e valor de dados](how-do-i-use-the-combobox-control-vb/_static/image6.jpg)](how-do-i-use-the-combobox-control-vb/_static/image11.png)

**Figura 06**: escolhendo os campos de texto e valor de dados ([clique para exibir a imagem em tamanho normal](how-do-i-use-the-combobox-control-vb/_static/image12.png))

Depois de concluir as etapas acima, a ComboBox é associada a um controle SqlDataSource que representa os filmes da tabela de banco de dados de filmes. A origem da página é semelhante à listagem 2 (eu limpei a formatação um pouco).

**Listagem 2-Movies. aspx**

[!code-aspx[Main](how-do-i-use-the-combobox-control-vb/samples/sample2.aspx)]

Observe que o controle ComboBox tem uma propriedade DataSourceID que aponta para o controle SqlDataSource. Quando você abre a página em um navegador, a lista de filmes do banco de dados é exibida (veja a Figura 7). Você pode escolher um filme na lista ou inserir um novo filme digitando o filme na ComboBox.

[![exibir uma lista de filmes](how-do-i-use-the-combobox-control-vb/_static/image7.jpg)](how-do-i-use-the-combobox-control-vb/_static/image13.png)

**Figura 07**: exibindo uma lista de filmes ([clique para exibir a imagem em tamanho normal](how-do-i-use-the-combobox-control-vb/_static/image14.png))

## <a name="setting-the-dropdownstyle"></a>Configurando o menu suspenso

Você pode usar a propriedade lista suspensa ComboBox para alterar o comportamento da ComboBox. Essa propriedade aceita os valores possíveis:

- DropDown-(valor padrão) a caixa de combinação exibe uma lista suspensa quando você clica na seta e pode inserir um valor personalizado.
- Simples-a caixa de combinação exibe uma lista suspensa automaticamente e você pode inserir um valor personalizado.
- DropDownList-a ComboBox funciona exatamente como um controle DropDownList.

O diferente entre DropDown e Simple é quando a lista de itens é exibida. No caso de simples, a lista é exibida imediatamente quando você move o foco para a caixa de combinação. No caso do menu suspenso, você deve clicar na seta para ver a lista de itens.

O valor DropDownList faz com que o controle ComboBox funcione exatamente como um controle DropDownList padrão. No entanto, há uma diferença importante aqui. As versões mais antigas do Internet Explorer exibem um controle DropDownList com um índice z infinito para que o controle apareça na frente de qualquer controle colocado na frente dele. Como a ComboBox renderiza uma marca HTML &lt;div&gt; em vez de uma &lt;HTML Select&gt; tag, a ComboBox respeita corretamente a ordenação z.

## <a name="setting-the-autocompletemode"></a>Configurando o AutoCompleteMode

Use a propriedade ComboBox AutoCompleteMode para especificar o que acontece quando alguém digita texto na ComboBox. Essa propriedade aceita os seguintes valores possíveis:

- Nenhum – (valor padrão) a ComboBox não fornece nenhum comportamento de preenchimento automático.
- Sugerir-a ComboBox exibe a lista e realça o item correspondente na lista (veja a Figura 8).
- Append-a ComboBox não exibe a lista e acrescenta o item correspondente da lista ao que você digitou (consulte a Figura 9).
- SuggestAppend-a ComboBox exibe a lista e acrescenta o item correspondente da lista para o que você digitou (consulte a Figura 10).

[![ComboBox faz uma sugestão](how-do-i-use-the-combobox-control-vb/_static/image8.jpg)](how-do-i-use-the-combobox-control-vb/_static/image15.png)

**Figura 08**: a ComboBox faz uma sugestão ([clique para exibir a imagem em tamanho normal](how-do-i-use-the-combobox-control-vb/_static/image16.png))

[![ComboBox acrescenta texto correspondente](how-do-i-use-the-combobox-control-vb/_static/image9.jpg)](how-do-i-use-the-combobox-control-vb/_static/image17.png)

**Figura 09**: a ComboBox acrescenta texto correspondente ([clique para exibir a imagem em tamanho normal](how-do-i-use-the-combobox-control-vb/_static/image18.png))

[![a ComboBox sugere e acrescenta](how-do-i-use-the-combobox-control-vb/_static/image10.jpg)](how-do-i-use-the-combobox-control-vb/_static/image19.png)

**Figura 10**: a ComboBox sugere e acrescenta ([clique para exibir a imagem em tamanho normal](how-do-i-use-the-combobox-control-vb/_static/image20.png))

## <a name="summary"></a>Resumo

Neste tutorial, você aprendeu a usar o controle ComboBox para exibir um conjunto fixo de itens. Vinculamos o controle ComboBox a um conjunto estático de itens e a uma tabela de banco de dados. Por fim, você aprendeu como modificar o comportamento da ComboBox definindo suas propriedades DropDownstyle e AutoCompleteMode.

> [!div class="step-by-step"]
> [Anterior](how-do-i-use-the-combobox-control-cs.md)
