---
uid: web-api/samples-list
title: Exemplos de API Web List-ASP.NET 4. x
author: rick-anderson
description: Lista de exemplos de ASP.NET Web API para ASP.NET 4. x
ms.author: riande
ms.date: 09/18/2012
ms.custom: seoapril2019
ms.assetid: 8cbd9d7f-7027-4390-b098-cb81a63ecd6f
msc.legacyurl: /web-api/samples-list
msc.type: content
ms.openlocfilehash: 24368fc80e7fc3c840f1f1f9accf13fa15a06c1b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598458"
---
# <a name="web-api-samples-list"></a>Lista de exemplos de API Web

## <a name="httpclient-samples"></a>Exemplos de HttpClient

**Exemplo de conversão do Bing** | [fonte vs 2012](https://github.com/aspnet/samples/blob/master/samples/aspnet/HttpClient/BingTranslateSample)

Mostra como chamar o [serviço Microsoft Translator](https://msdn.microsoft.com/library/ff512419.aspx) usando a classe **HttpClient** . A API do serviço Microsoft Translator requer um token OAuth, que o aplicativo obtém enviando uma solicitação ao servidor de token do Azure para cada solicitação ao serviço do tradutor. O resultado do servidor de token é colocado na solicitação enviada ao serviço de tradução. Antes de executar este exemplo, você deve obter uma [chave de aplicativo do Azure Marketplace](https://msdn.microsoft.com/library/hh454950.aspx) e preencher as informações na classe de exemplo AccessTokenMessageHandler.

**Exemplo do Google Maps** | [Descrição detalhada](https://blogs.msdn.com/b/henrikn/archive/2012/02/17/downloading-a-google-map-to-local-file.aspx) | [fonte vs 2012](https://github.com/aspnet/samples/blob/master/samples/aspnet/HttpClient/GoogleMapsSample)

Usa o **HttpClient** para baixar um mapa de Redmond, WA da [API do Google Maps](https://developers.google.com/maps/), salva-o como um arquivo local e abre o Visualizador de imagem padrão.

**Exemplo de cliente do Twitter** | [Descrição detalhada](https://blogs.msdn.com/b/henrikn/archive/2012/02/16/extending-httpclient-with-oauth-to-access-twitter.aspx) | [fonte vs 2012](https://github.com/aspnet/samples/blob/master/samples/aspnet/HttpClient/TwitterSample)

Mostra como escrever um cliente do Twitter simples usando o **HttpClient**. O exemplo usa um **HttpMessageHandler** para inserir informações de autenticação OAuth no **HttpRequestMessage**de saída. O resultado do Twitter é lido usando JSON.NET. Antes de executar este exemplo, você deve obter uma [chave de aplicativo do Twitter](https://dev.twitter.com/)e preencher as informações na classe de exemplo OAuthMessageHandler.

**Exemplo de banco mundial** | [Descrição detalhada](https://blogs.msdn.com/b/henrikn/archive/2012/02/16/httpclient-is-here.aspx) | [fonte do vs 2010](https://github.com/aspnet/samples/blob/master/samples/aspnet/HttpClient/WorldBankSample/Net40) | fonte [vs 2012](https://github.com/aspnet/samples/blob/master/samples/aspnet/HttpClient/WorldBankSample/Net45)

Mostra como recuperar dados do site de dados do World Bank, usando JSON.NET para analisar o resultado.

## <a name="web-api-samples"></a>Exemplos de API Web

**Introdução com ASP.NET Web API** [fonte | vs 2012](overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)

Mostra como criar uma API Web básica que dá suporte a solicitações HTTP GET. Contém o código-fonte do tutorial que [seu primeiro ASP.NET Web API](overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).

**Cenários de ASP.NET Web API JavaScript – comentários** | [fonte vs 2012](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7)

Mostra como usar ASP.NET Web API para criar APIs Web que dão suporte a clientes de navegador e podem ser facilmente chamados usando o jQuery.

**Gerenciador de contatos** | [fonte vs 2010](https://code.msdn.microsoft.com/Contact-Manager-Web-API-0e8e373d)

Este exemplo usa ASP.NET Web API para criar um aplicativo simples de Gerenciador de contatos. O aplicativo consiste em uma API Web do Gerenciador de contatos que é usada por um aplicativo MVC ASP.NET e um aplicativo Windows Phone para exibir e gerenciar uma lista de contatos.

**Exemplo de envio em lote** | [Descrição detalhada](http://trocolate.wordpress.com/2012/07/19/mitigate-issue-260-in-batching-scenario/) | [fonte vs 2012](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/BatchSample)

Mostra como implementar o envio em lote HTTP no ASP.NET. O envio em lote consiste em colocar várias solicitações HTTP em um único corpo de entidade MIME com várias partes, que é enviado para o servidor como um HTTP POST. As solicitações são processadas individualmente e as respostas são colocadas em outro corpo de entidade de várias partes MIME, que é retornado ao cliente.

**Exemplo de controlador de conteúdo** | [Descrição detalhada](https://blogs.msdn.com/b/henrikn/archive/2012/02/24/async-actions-in-asp-net-web-api.aspx) | [fonte do vs 2010](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/ContentControllerSample/Net40) | fonte [vs 2012](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/ContentControllerSample/Net45)

Mostra como ler e gravar entidades de solicitação e resposta de forma assíncrona usando fluxos. O controlador de exemplo tem duas ações: uma ação PUT que lê o corpo da entidade de solicitação de forma assíncrona e armazena-o em um arquivo local, e uma ação GET que retorna o conteúdo do arquivo local.

**Exemplo de resolvedor de assembly personalizado** | [fonte vs 2012](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/CustomAssemblyResolverSample)

Mostra como modificar ASP.NET Web API para dar suporte à descoberta de controladores de um assembly de biblioteca carregado dinamicamente. O exemplo implementa um **IAssembliesResolver** personalizado que chama a implementação padrão e, em seguida, adiciona o assembly da biblioteca aos resultados padrão.

**Exemplo de formatador de tipo de mídia personalizado** | [Descrição detalhada](https://blogs.msdn.com/b/henrikn/archive/2012/04/23/using-cookies-with-asp-net-web-api.aspx) | [fonte vs 2010](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/CustomMediaTypeFormatterSample)

Mostra como criar um formatador de tipo de mídia personalizado usando a classe base **BufferedMediaTypeFormatter** . Essa classe base destina-se a formatadores que usam principalmente operações síncronas de leitura e gravação. Além de mostrar o formatador de tipo de mídia, o exemplo mostra como conectá-lo registrando-o como parte do **HttpConfiguration** para seu aplicativo. Observe que também é possível usar a classe base **MediaTypeFormatter** diretamente, para formatadores que usam principalmente operações assíncronas de leitura e gravação.

**Exemplo de associação de parâmetro personalizado** | [Descrição detalhada](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx) | [fonte vs 2010](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/CustomParameterBinding)

Mostra como personalizar o processo de associação de parâmetro, que é o processo que determina como as informações de uma solicitação são associadas a parâmetros de ação. Neste exemplo, o controlador inicial tem quatro ações:

1. BindPrincipal mostra como associar um parâmetro IPrincipal de uma entidade de segurança genérica personalizada, não de uma mensagem HTTP GET;
2. BindCustomComplexTypeFromUriOrBody mostra como associar um parâmetro de tipo complexo, que pode vir do corpo da mensagem ou do URI de solicitação de uma mensagem HTTP POST;
3. BindCustomComplexTypeFromUriWithRenamedProperty mostra como associar um parâmetro de tipo complexo a uma propriedade renomeada que vem do URI de solicitação de uma mensagem HTTP POST;
4. PostMultipleParametersFromBody mostra como associar vários parâmetros do corpo de uma mensagem POST;

**Exemplo de upload de arquivo** | [Descrição detalhada](https://blogs.msdn.com/b/henrikn/archive/2012/03/01/file-upload-and-asp-net-web-api.aspx) | [fonte vs 2012](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/FileUploadSample)

Mostra como carregar arquivos para um **ApiController** usando o carregamento de arquivo com várias partes MIME e como configurar notificações de progresso com o **HttpClient** usando **ProgressNotificationHandler**. O controlador lê o conteúdo de um arquivo HTML carregar de forma assíncrona e grava uma ou mais partes do corpo em um arquivo local. A resposta contém informações sobre o arquivo (ou arquivos) carregado.

**Exemplo de upload de arquivo para repositório de blob do Azure** | [Descrição detalhada](https://blogs.msdn.com/b/yaohuang1/archive/2012/07/02/asp-net-web-api-and-azure-blob-storage.aspx) | [fonte vs 2012](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/AzureBlobsFileUploadSample)

Este exemplo é semelhante ao exemplo de upload de arquivo, mas em vez de salvar os arquivos carregados no disco local, ele carrega assincronamente os arquivos no [repositório de blob do Azure](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs) usando o [SDK do Windows Azure para .net](https://www.windowsazure.com/develop/net/). Ele também fornece um mecanismo para listar os BLOBs atualmente presentes em um [contêiner de armazenamento de BLOBs do Azure](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs). Você pode experimentar o exemplo em execução no **emulador de armazenamento do Azure** que acompanha o SDK do Azure. Se você tiver uma [conta de armazenamento do Azure](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs), também poderá executar no serviço de armazenamento real.

**Exemplo de pipeline do manipulador de mensagens Http** | [Descrição detalhada](https://blogs.msdn.com/b/henrikn/archive/2012/08/07/httpclient-httpclienthandler-and-httpwebrequesthandler.aspx) | [fonte vs 2010](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/HttpMessageHandlerPipelineSample)

Mostra como conectar instâncias de **HttpMessageHandler** tanto no cliente (**HttpClient**) quanto no servidor (ASP.NET Web API). No exemplo, o mesmo manipulador é usado no cliente e no servidor. Embora seja raro que exatamente o mesmo manipulador seja executado em ambos os locais, o modelo de objeto é o mesmo no lado do cliente e do servidor.

**Exemplo de carregamento JSON** | [fonte vs 2012](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/JsonUploadSample)

Mostra como carregar e baixar o JSON de e para um **ApiController**. O exemplo usa um mínimo de **ApiController** e acessa-o usando **HttpClient**.

**Exemplo de Mashup** | [Descrição detalhada](https://blogs.msdn.com/b/henrikn/archive/2012/03/03/async-mashups-using-asp-net-web-api.aspx) | [fonte vs 2012](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/MashupSample)

Mostra como acessar vários sites remotos de forma assíncrona de dentro de uma ação **ApiController** . Cada vez que a ação é atingida, as solicitações são executadas de forma assíncrona, para que nenhum thread seja bloqueado.

**Exemplo de rastreamento de memória** | [Descrição detalhada](https://blogs.msdn.com/b/roncain/archive/2012/04/12/tracing-in-asp-net-web-api.aspx) | [fonte vs 2010](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/MemoryTracingSample)

Este projeto de exemplo cria um pacote NuGet que instalará um gravador de rastreamento na memória personalizado em aplicativos ASP.NET Web API.

**Exemplo do MongoDB** | [Descrição detalhada](https://blogs.msdn.com/b/henrikn/archive/2012/02/19/using-web-api-with-mongodb.aspx) | [fonte vs 2012](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/MongoSample)

Mostra como usar o MongoDB como o armazenamento persistente para um **ApiController**, usando um padrão de repositório.

**Exemplo de processador de corpo de resposta** | [fonte vs 2012](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/ResponseEntityProcessorSample)

Mostra como copiar uma entidade de resposta (ou seja, um corpo de resposta HTTP) para um arquivo local antes que ele seja transmitido para o cliente e executar processamento adicional nesse arquivo de forma assíncrona. O exemplo implementa um **HttpMessageHandler** que encapsula a entidade de resposta com uma que ambos grava na saída como normal e em um arquivo local.

**Carregar exemplo de XDocument** | [Descrição detalhada](https://blogs.msdn.com/b/henrikn/archive/2012/02/17/push-and-pull-streams-using-httpclient.aspx) | [fonte vs 2012](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/UploadXDocumentSample)

Mostra como carregar um XDocument para um **ApiController** usando **PushStreamContent** e **HttpClient**.

**Exemplo de validação** | [fonte vs 2010](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/ValidationSample)

Mostra como você pode usar atributos de validação em seus modelos no ASP.NET WebAPI para validar o conteúdo da solicitação HTTP. Demonstra como marcar as propriedades conforme necessário, como usar os atributos de validação personalizados e definidos pelo Framework para anotar seu modelo e como retornar respostas de erro para Estados de modelo inválidos.

**Exemplo de formulário da Web** | [Descrição detalhada](https://blogs.msdn.com/b/henrikn/archive/2012/02/23/using-asp-net-web-api-with-asp-net-web-forms.aspx) | [fonte vs 2010](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/WebFormSample)

Mostra um ApiController adicionado a um projeto Web Forms.

**[Exemplo de RestBugs](https://github.com/howarddierking/RestBugs)**

RestBugs é um aplicativo de rastreamento de bugs simples que mostra como usar ASP.NET Web API e a nova biblioteca de cliente HTTP para criar um sistema orientado por hipermídia. O exemplo inclui implementações de cliente e servidor, usando ASP.NET Web API. O servidor usa um formatador do Razor personalizado para gerar representações de recursos. O exemplo também fornece um servidor node. js para ilustrar os benefícios que vêm do uso de um design hipermídia para desacoplar clientes e servidores.
