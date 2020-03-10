---
uid: web-api/overview/advanced/sending-html-form-data-part-2
title: 'Enviando dados de formulário HTML no ASP.NET Web API: carregamento de arquivo e MIME de várias partes-ASP.NET 4. x'
author: MikeWasson
description: Este tutorial mostra como carregar arquivos para uma API da Web. Ele também descreve como processar dados MIME de várias partes.
ms.author: riande
ms.date: 06/21/2012
ms.custom: seoapril2019
ms.assetid: a7f3c1b5-69d9-4261-b082-19ffafa5f16a
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-2
msc.type: authoredcontent
ms.openlocfilehash: f5aaebb96f631dfb6b0da1fbca96cd93a6a7fe2d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557564"
---
# <a name="sending-html-form-data-in-aspnet-web-api-file-upload-and-multipart-mime"></a>Enviando dados de formulário HTML no ASP.NET Web API: carregamento de arquivo e MIME de várias partes

por [Mike Wasson](https://github.com/MikeWasson)

## <a name="part-2-file-upload-and-multipart-mime"></a>Parte 2: carregamento de arquivo e MIME de várias partes

Este tutorial mostra como carregar arquivos para uma API da Web. Ele também descreve como processar dados MIME de várias partes.

> [!NOTE]
> [Baixe o projeto concluído](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).

Aqui está um exemplo de um formulário HTML para carregar um arquivo:

[!code-html[Main](sending-html-form-data-part-2/samples/sample1.html)]

![](sending-html-form-data-part-2/_static/image1.png)

Esse formulário contém um controle de entrada de texto e um controle de entrada de arquivo. Quando um formulário contém um controle de entrada de arquivo, o atributo **Enctype** sempre deve ser &quot;&quot;de dados multipart/formulário, o que especifica que o formulário será enviado como uma mensagem MIME de várias partes.

O formato de uma mensagem MIME de várias partes é mais fácil de entender examinando uma solicitação de exemplo:

[!code-console[Main](sending-html-form-data-part-2/samples/sample2.cmd)]

Essa mensagem é dividida em duas *partes*, uma para cada controle de formulário. Os limites de parte são indicados pelas linhas que começam com traços.

> [!NOTE]
> O limite de parte inclui um componente aleatório (&quot;41184676334&quot;) para garantir que a cadeia de caracteres de limite não apareça acidentalmente dentro de uma parte de mensagem.

Cada parte da mensagem contém um ou mais cabeçalhos, seguidos pelo conteúdo da parte.

- O cabeçalho de disposição de conteúdo inclui o nome do controle. Para arquivos, ele também contém o nome do arquivo.
- O cabeçalho Content-Type descreve os dados na parte. Se esse cabeçalho for omitido, o padrão será text/plain.

No exemplo anterior, o usuário carregou um arquivo chamado GrandCanyon. jpg, com o tipo de conteúdo image/jpeg; e o valor da entrada de texto foi &quot;férias de verão&quot;.

## <a name="file-upload"></a>Carregar arquivos

Agora, vamos examinar um controlador de API da Web que lê arquivos de uma mensagem MIME de várias partes. O controlador lerá os arquivos de forma assíncrona. A API Web dá suporte a ações assíncronas usando o [modelo de programação baseado em tarefas](https://msdn.microsoft.com/library/dd460693.aspx). Primeiro, este é o código se você estiver direcionando .NET Framework 4,5, que oferece suporte às palavras-chave **Async** e **Await** .

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample3.cs)]

Observe que a ação do controlador não assume nenhum parâmetro. Isso porque processamos o corpo da solicitação dentro da ação, sem invocar um formatador de tipo de mídia.

O método **IsMultipartContent** verifica se a solicitação contém uma mensagem MIME de várias partes. Caso contrário, o controlador retornará o código de status HTTP 415 (tipo de mídia sem suporte).

A classe **MultipartFormDataStreamProvider** é um objeto auxiliar que aloca fluxos de arquivos para arquivos carregados. Para ler a mensagem MIME de várias partes, chame o método **ReadAsMultipartAsync** . Esse método extrai todas as partes da mensagem e as grava nos fluxos fornecidos pelo **MultipartFormDataStreamProvider**.

Quando o método for concluído, você poderá obter informações sobre os arquivos da propriedade **FileData** , que é uma coleção de objetos **MultipartFileData** .

- **MultipartFileData. FileName** é o nome do arquivo local no servidor em que o arquivo foi salvo.
- **MultipartFileData. Headers** contém o cabeçalho da parte (*não* o cabeçalho da solicitação). Você pode usar isso para acessar o conteúdo\_os cabeçalhos de disposição e tipo de conteúdo.

Como o nome sugere, **ReadAsMultipartAsync** é um método assíncrono. Para executar o trabalho após a conclusão do método, use uma [tarefa de continuação](https://msdn.microsoft.com/library/ee372288.aspx) (.NET 4,0) ou a palavra-chave **await** (.NET 4,5).

Aqui está a versão .NET Framework 4,0 do código anterior:

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample4.cs)]

## <a name="reading-form-control-data"></a>Lendo dados de controle de formulário

O formulário HTML que mostrei anteriormente tinha um controle de entrada de texto.

[!code-html[Main](sending-html-form-data-part-2/samples/sample5.html)]

Você pode obter o valor do controle da propriedade **FormData** do **MultipartFormDataStreamProvider**.

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample6.cs?highlight=15)]

**FormData** é uma **NameValueCollection** que contém pares de nome/valor para os controles de formulário. A coleção pode conter chaves duplicadas. Considere este formulário:

[!code-html[Main](sending-html-form-data-part-2/samples/sample7.html)]

![](sending-html-form-data-part-2/_static/image2.png)

O corpo da solicitação pode ser assim:

[!code-console[Main](sending-html-form-data-part-2/samples/sample8.cmd)]

Nesse caso, a coleção **FormData** conterá os seguintes pares de chave/valor:

- viagem: ida e volta
- opções: Nonstop
- opções: datas
- Estação: janela
