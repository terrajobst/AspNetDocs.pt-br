---
uid: web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
title: Integrando a interface do usuário do JQuery DatePicker com associação de modelo e formulários da Web | Microsoft Docs
author: Rick-Anderson
description: Esta série de tutoriais demonstra os aspectos básicos do uso de associação de modelo com um projeto ASP.NET Web Forms. A associação de modelo torna a interação de dados mais direta-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 3cbab37b-fb0f-4751-9ec4-74e068c3f380
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: c8d711dd44950116f3a3e09d5d12c507918c543f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78642474"
---
# <a name="integrating-jquery-ui-datepicker-with-model-binding-and-web-forms"></a>Integrando a interface do usuário do JQuery DatePicker com associação de modelo e formulários da Web

por [Tom FitzMacken](https://github.com/tfitzmac)

> Esta série de tutoriais demonstra os aspectos básicos do uso de associação de modelo com um projeto ASP.NET Web Forms. A associação de modelo torna a interação de dados mais direta do que lidar com objetos de fonte de dados (como ObjectDataSource ou SqlDataSource). Esta série começa com material introdutório e passa para conceitos mais avançados em Tutoriais posteriores.
> 
> Este tutorial mostra como adicionar o [widget DatePicker](http://jqueryui.com/datepicker/) da interface do usuário do jQuery a um formulário da Web e usar a associação de modelo para atualizar o banco de dados com o valor selecionado.
> 
> Este tutorial se baseia no projeto criado na [primeira](retrieving-data.md) e na [segunda](updating-deleting-and-creating-data.md) parte da série.
> 
> Você pode [baixar](https://go.microsoft.com/fwlink/?LinkId=286116) o projeto completo no C# ou no VB. O código para download funciona com o Visual Studio 2012 ou Visual Studio 2013. Ele usa o modelo do Visual Studio 2012, que é um pouco diferente do modelo de Visual Studio 2013 mostrado neste tutorial.

## <a name="what-youll-build"></a>O que você criará

Neste tutorial, você aprenderá a:

1. Adicionar uma propriedade ao modelo para registrar a data de registro do aluno
2. Habilitar o usuário a selecionar a data de registro usando o widget DatePicker da interface do usuário do JQuery
3. Impor regras de validação para a data de registro

O widget DatePicker da interface do usuário do JQuery permite que os usuários selecionem facilmente uma data de um calendário que aparece quando o usuário interage com o campo. O uso desse widget pode ser mais conveniente para usuários do que digitar manualmente uma data. A integração do widget DatePicker em uma página que usa associação de modelo para operações de dados requer apenas uma pequena quantidade de trabalho adicional.

## <a name="add-a-new-property-to-the-model"></a>Adicionar uma nova propriedade ao modelo

Primeiro, você adicionará uma propriedade **DateTime** ao modelo de aluno e migrará essa alteração para o banco de dados. Abra **UniversityModels.cs**e adicione o código realçado ao modelo de aluno.

[!code-csharp[Main](integrating-jquery-ui/samples/sample1.cs?highlight=16-18)]

**RangeAttribute** é incluído para impor regras de validação para a propriedade. Para este tutorial, vamos pressupor que a Contoso University foi fundada em 1º de janeiro de 2013 e, portanto, as datas de registro anteriores não são válidas.

Na janela Gerenciamento de Pacotes, adicione uma migração executando o comando **Add-Migration AddEnrollmentDate**. Observe que o código de migração adiciona a nova coluna datetime à tabela Student. Para corresponder ao valor especificado no RangeAttribute, adicione um valor padrão para a nova coluna, conforme mostrado no código realçado abaixo.

[!code-csharp[Main](integrating-jquery-ui/samples/sample2.cs?highlight=11)]

Salve a alteração no arquivo de migração.

Você não precisa propagar os dados novamente. Portanto, abra **Configuration.cs** na pasta migrações e remova ou comente o código no método **semente** . Salve e feche o arquivo.

Agora, execute o comando **Update-Database**. Observe que a coluna agora existe no banco de dados e todos os registros existentes têm o valor padrão para EnrollmentDate.

## <a name="add-dynamic-controls-for-enrollment-date"></a>Adicionar controles dinâmicos para data de registro

Agora, você adicionará controles para exibir e editar a data de registro. Neste ponto, o valor é editado por meio de uma caixa de texto. Posteriormente no tutorial, você alterará a caixa de texto para o widget DatePicker do JQuery.

Primeiro, é importante observar que você não precisa fazer nenhuma alteração no arquivo **addstudent. aspx** . O controle DynamicEntity irá renderizar automaticamente a nova propriedade.

Abra o **students. aspx**e adicione o código realçado a seguir.

[!code-aspx[Main](integrating-jquery-ui/samples/sample3.aspx?highlight=13)]

Execute o aplicativo e observe que você pode definir o valor da data de registro digitando uma data. Ao adicionar um novo aluno:

![Definir data](integrating-jquery-ui/_static/image1.png)

Ou editando um valor existente:

![editar data](integrating-jquery-ui/_static/image2.png)

Digitar a data funciona, mas pode não ser a experiência do cliente que você deseja fornecer. Na próxima seção, você permitirá selecionar uma data por meio de um calendário.

## <a name="install-nuget-package-to-work-with-jquery-ui"></a>Instalar o pacote NuGet para trabalhar com a interface do usuário do JQuery

O pacote NuGet **da interface do usuário do suco** permite uma fácil integração dos widgets da interface do usuário do jQuery em seu aplicativo Web. Para usar este pacote, instale-o por meio do NuGet.

![Adicionar interface do usuário do suco](integrating-jquery-ui/_static/image3.png)

A versão da interface do usuário do suco que você instala pode entrar em conflito com a versão do JQuery em seu aplicativo. Antes de prosseguir com este tutorial, tente executar o aplicativo. Se você encontrar um erro de JavaScript, precisará reconciliar a versão do JQuery. Você pode adicionar a versão esperada do JQuery à sua pasta de scripts (versão 1.8.2 no momento de escrever este tutorial) ou em site. Master especifique o caminho para o arquivo JQuery.

[!code-aspx[Main](integrating-jquery-ui/samples/sample4.aspx)]

## <a name="customize-datetime-template-to-include-datepicker-widget"></a>Personalizar o modelo DateTime para incluir o widget DatePicker

Você adicionará o widget DatePicker ao modelo de dados dinâmico para editar um valor DateTime. Ao adicionar o widget ao modelo, ele é automaticamente renderizado no formulário para adicionar um novo aluno e no modo de exibição de grade para edição de alunos. Abra **DateTime\_Edit. ascx**e adicione o seguinte código realçado.

[!code-aspx[Main](integrating-jquery-ui/samples/sample5.aspx?highlight=3)]

No arquivo code-behind, você definirá as datas mínima e máxima para o DatePicker. Ao definir esses valores, você impedirá que os usuários naveguem até datas inválidas. Você recuperará os valores mínimo e máximo do **intervaloattribute** na propriedade DateTime, se um for fornecido. Abra **DateTime\_Edit.ascx.cs**e adicione o seguinte código realçado à página\_método de carregamento.

[!code-csharp[Main](integrating-jquery-ui/samples/sample6.cs?highlight=9-14)]

Execute o aplicativo Web e navegue até a página addaluno. Forneça valores para os campos e observe que, quando você clica na caixa de texto para data de inscrição, o calendário é exibido.

![seletor de data](integrating-jquery-ui/_static/image4.png)

Escolha uma data e clique em **Inserir**. O RangeAttribute impõe a validação no servidor. Ao definir a propriedade mental no DatePicker, você também aplicará a validação no cliente. O calendário não permite que o usuário navegue até uma data anterior ao valor de mente.

Quando você edita um registro no modo de exibição de grade, o calendário também é exibido.

![DatePicker em GridView](integrating-jquery-ui/_static/image5.png)

## <a name="conclusion"></a>Conclusão

Neste tutorial, você aprendeu a incorporar um widget do JQuery em um formulário da Web que usa a associação de modelo.

No próximo [tutorial](using-query-string-values-to-retrieve-data.md), você usará um valor de cadeia de caracteres de consulta ao selecionar dados.

> [!div class="step-by-step"]
> [Anterior](sorting-paging-and-filtering-data.md)
> [Próximo](using-query-string-values-to-retrieve-data.md)
