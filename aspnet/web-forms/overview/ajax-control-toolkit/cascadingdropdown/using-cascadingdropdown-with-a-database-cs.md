---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-cs
title: Usando CascadingDropDown com um banco deC#dados () | Microsoft Docs
author: wenz
description: O controle CascadingDropDown no AJAX Control Toolkit estende um controle DropDownList para que as alterações em uma DropDownList carreguem valores associados em anoth...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 684f0c28-a490-4e5b-b5e5-5dfb77464b49
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-cs
msc.type: authoredcontent
ms.openlocfilehash: bcf453170d17807b4e3b2d2a8b545cba43139f89
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78597856"
---
# <a name="using-cascadingdropdown-with-a-database-c"></a>Uso de CascadingDropDown com um banco de dados (C#)

por [Christian Wenz](https://github.com/wenz)

[Baixar código](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown1.cs.zip) ou [baixar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown1CS.pdf)

> O controle CascadingDropDown no AJAX Control Toolkit estende um controle DropDownList para que as alterações em uma DropDownList carreguem valores associados em outra DropDownList. Para que isso funcione, um serviço Web especial deve ser criado.

## <a name="overview"></a>Visão geral

O controle CascadingDropDown no AJAX Control Toolkit estende um controle DropDownList para que as alterações em uma DropDownList carreguem valores associados em outra DropDownList. (Por exemplo, uma lista fornece uma lista de Estados dos EUA e a próxima lista é então preenchida com cidades principais nesse estado.) Para que isso funcione, um serviço Web especial deve ser criado.

## <a name="steps"></a>Etapas

Em primeiro lugar, uma fonte de dados é necessária. Este exemplo usa o banco de dados AdventureWorks e o Microsoft SQL Server 2005 Express Edition. O banco de dados é uma parte opcional de uma instalação do Visual Studio (incluindo a edição Express) e também está disponível como um download separado em [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064). O banco de dados AdventureWorks faz parte dos exemplos SQL Server 2005 e bancos de dados de exemplo (download em [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). A maneira mais fácil de definir o banco de dados é usar o Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) e anexar o arquivo de banco de dados `AdventureWorks.mdf`.

Para este exemplo, presumimos que a instância do SQL Server 2005 Express Edition é chamada `SQLEXPRESS` e reside no mesmo computador que o servidor Web; Essa também é a configuração padrão. Se a configuração for diferente, você precisará adaptar as informações de conexão do banco de dados.

Para ativar a funcionalidade do ASP.NET AJAX e do kit de ferramentas de controle, o controle de `ScriptManager` deve ser colocado em qualquer lugar na página (mas dentro do elemento &lt;`form`&gt;):

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample1.aspx)]

Na próxima etapa, são necessários dois controles DropDownList. Neste exemplo, usamos o fornecedor e as informações de contato do AdventureWorks, portanto, criamos uma lista para os fornecedores disponíveis e um para os contatos disponíveis:

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample2.aspx)]

Em seguida, dois extensores CascadingDropDown devem ser adicionados à página. Um preenche a primeira lista de (fornecedores) e a outra preenche a segunda lista (contatos). Os atributos a seguir devem ser definidos:

- `ServicePath`: URL de um serviço Web que fornece as entradas da lista
- `ServiceMethod`: método Web fornecendo as entradas da lista
- `TargetControlID`: ID da lista suspensa
- `Category`: informações de categoria que são enviadas para o método Web quando chamado
- `PromptText`: texto exibido ao carregar dados da lista de forma assíncrona do servidor
- `ParentControlID`: (opcional) lista suspensa pai que dispara o carregamento da lista atual

Dependendo da linguagem de programação usada, o nome do serviço Web em questão é alterado, mas todos os outros valores de atributo são os mesmos. Aqui está o elemento CascadingDropDown para a primeira lista suspensa:

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample3.aspx)]

Os extensores de controle da segunda lista precisam definir o atributo `ParentControlID` para que a seleção de uma entrada na lista fornecedores acione o carregamento de elementos associados na lista de contatos.

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample4.aspx)]

O trabalho real é então feito no serviço Web, que é configurado da seguinte maneira. Observe que o atributo `[ScriptService]` é usado; caso contrário, o ASP.NET AJAX não pode criar o proxy JavaScript para acessar os métodos da Web do código de script do lado do cliente.

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample5.aspx)]

A assinatura dos métodos da Web chamados por CascadingDropDown é a seguinte:

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample6.cs)]

Portanto, o valor de retorno deve ser uma matriz do tipo `CascadingDropDownNameValue` que é definida pelo kit de ferramentas de controle. O método `GetVendors()` é bem fácil de implementar: o código se conecta ao banco de dados AdventureWorks e consulta os primeiros 25 fornecedores. O primeiro parâmetro no construtor de `CascadingDropDownNameValue` é a legenda da entrada da lista, o segundo seu valor (atributo de valor no &lt;do elemento HTML `option`&gt;). Eis o código:

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample7.cs)]

Obter os contatos associados para um fornecedor (nome do método: `GetContactsForVendor()`) é um pouco mais complicado. Em primeiro lugar, o fornecedor selecionado na primeira lista suspensa deve ser determinado. O kit de ferramentas de controle define um método auxiliar para essa tarefa: o método `ParseKnownCategoryValuesString()` retorna um elemento `StringDictionary` com os dados suspensos:

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample8.cs)]

Por motivos de segurança, esses dados devem ser validados primeiro. Portanto, se houver uma entrada de fornecedor (porque a propriedade `Category` do primeiro elemento CascadingDropDown está definida como `"Vendor"`), a ID do fornecedor selecionado poderá ser recuperada:

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample9.cs)]

O restante do método é bastante direto, então. A ID do fornecedor é usada como um parâmetro para uma consulta SQL que recupera todos os contatos associados para esse fornecedor. Mais uma vez, o método retorna uma matriz do tipo `CascadingDropDownNameValue`.

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample10.cs)]

Carregue a página ASP.NET e, após um curto período, a lista de fornecedores será preenchida com 25 entradas. Escolha uma entrada e observe como a segunda lista suspensa é preenchida com dados.

[![a primeira lista é preenchida automaticamente](using-cascadingdropdown-with-a-database-cs/_static/image2.png)](using-cascadingdropdown-with-a-database-cs/_static/image1.png)

A primeira lista é preenchida automaticamente ([clique para exibir a imagem em tamanho normal](using-cascadingdropdown-with-a-database-cs/_static/image3.png))

[![a segunda lista é preenchida de acordo com a seleção na primeira lista](using-cascadingdropdown-with-a-database-cs/_static/image5.png)](using-cascadingdropdown-with-a-database-cs/_static/image4.png)

A segunda lista é preenchida de acordo com a seleção na primeira lista ([clique para exibir a imagem em tamanho normal](using-cascadingdropdown-with-a-database-cs/_static/image6.png))

> [!div class="step-by-step"]
> [Anterior](filling-a-list-using-cascadingdropdown-cs.md)
> [Próximo](presetting-list-entries-with-cascadingdropdown-cs.md)
