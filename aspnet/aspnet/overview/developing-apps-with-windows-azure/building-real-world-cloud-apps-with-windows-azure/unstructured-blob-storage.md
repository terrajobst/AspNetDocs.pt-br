---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
title: Armazenamento de BLOBs não estruturado (compilando aplicativos de nuvem do mundo real com o Azure) | Microsoft Docs
author: MikeWasson
description: A criação de aplicativos de nuvem do mundo real com o livro eletrônico do Azure é baseada em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas que podem...
ms.author: riande
ms.date: 03/30/2015
ms.assetid: 9f05ccb1-2004-4661-ad8b-c370e6c09c8e
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
msc.type: authoredcontent
ms.openlocfilehash: 2afd4b5cf640eb97080de7e5280409f5e5347731
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74583634"
---
# <a name="unstructured-blob-storage-building-real-world-cloud-apps-with-azure"></a>Armazenamento de BLOBs não estruturado (compilando aplicativos de nuvem do mundo real com o Azure)

por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Baixar o projeto de ti](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [baixar o livro eletrônico](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> A **criação de aplicativos de nuvem do mundo real com** o livro eletrônico do Azure é baseada em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas que podem ajudá-lo a desenvolver com sucesso aplicativos Web para a nuvem. Para obter informações sobre o livro eletrônico, consulte [o primeiro capítulo](introduction.md).

No capítulo anterior, examinamos os esquemas de particionamento e explicamos como o aplicativo Fix it armazena as imagens no serviço de Azure Storage Blob e outros dados de tarefa no Azure SQL Database. Neste capítulo, vamos detalhar o serviço BLOB e mostrar como ele é implementado em corrigir o código do projeto.

## <a name="what-is-blob-storage"></a>O que é o armazenamento de BLOBs?

O serviço de Azure Storage Blob fornece uma maneira de armazenar arquivos na nuvem. O serviço blob tem várias vantagens em relação ao armazenamento de arquivos em um sistema de arquivos de rede local:

- É altamente escalonável. Uma única conta de armazenamento pode armazenar [centenas de terabytes](https://msdn.microsoft.com/library/windowsazure/dn249410.aspx), e você pode ter várias contas de armazenamento. Alguns dos maiores clientes do Azure armazenam centenas de petabytes. O Microsoft SkyDrive usa o armazenamento de BLOBs.
- É durável. Todo arquivo que você armazena no serviço blob é submetido a backup automaticamente.
- Ele fornece alta disponibilidade. O [SLA para armazenamento](https://go.microsoft.com/fwlink/p/?linkid=159705&amp;clcid=0x409) promete 99,9% ou 99,99% de tempo de atividade, dependendo de qual opção de redundância geográfica você escolher.
- É um recurso de PaaS (plataforma como serviço) do Azure, o que significa que você apenas armazena e recupera arquivos, pagando apenas pela quantidade real de armazenamento que você usa, e o Azure cuida automaticamente da configuração e do gerenciamento de todas as VMs e unidades de disco necessárias para o serviço.
- Você pode acessar o serviço BLOB usando uma API REST ou usando uma API de linguagem de programação. Os SDKs estão disponíveis para .NET, Java, Ruby e outros.
- Ao armazenar um arquivo no serviço BLOB, você pode torná-lo publicamente disponível pela Internet.
- Você pode proteger arquivos no serviço blob para que eles possam ser acessados somente por usuários autorizados, ou você pode fornecer tokens de acesso temporários que os tornam disponíveis para alguém somente por um período de tempo limitado.

Sempre que você estiver criando um aplicativo para o Azure e quiser armazenar muitos dados que em um ambiente local entrarão em arquivos, como imagens, vídeos, PDFs, planilhas, etc., considere o serviço BLOB.

## <a name="creating-a-storage-account"></a>Criando uma conta de armazenamento

Para começar a usar o serviço BLOB, você cria uma conta de armazenamento no Azure. No portal, clique em **novo** -- **serviços de dados** -- **armazenamento** -- **criação rápida**e, em seguida, insira uma URL e um local de data center. O local de data center deve ser o mesmo que seu aplicativo Web.

![Criar uma conta de armazenamento](unstructured-blob-storage/_static/image1.png)

Você escolhe a região primária em que deseja armazenar o conteúdo e, se escolher a opção de [replicação geográfica](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx#_Geo_Redundant_Storage) , o Azure criará réplicas de todos os seus dados em um Data Center diferente em outra região do país. Por exemplo, se você escolher o data center oeste dos EUA, ao armazenar um arquivo, ele vai para o data center ocidental dos EUA, mas, em segundo plano, o Azure também o copia para um dos outros data centers dos EUA. Se um desastre ocorrer em uma região do país, seus dados ainda serão seguros.

O Azure não replicará dados entre limites de política geográfica: se o seu local principal estiver nos EUA, seus arquivos serão replicados somente para outra região dentro dos EUA; Se o seu local principal for Austrália, seus arquivos serão replicados somente para outro data center na Austrália.

É claro que você também pode criar uma conta de armazenamento executando comandos de um script, como vimos anteriormente. Aqui está um comando do Windows PowerShell para criar uma conta de armazenamento:

[!code-powershell[Main](unstructured-blob-storage/samples/sample1.ps1)]

Depois de ter uma conta de armazenamento, você pode iniciar imediatamente o armazenamento de arquivos no serviço BLOB.

## <a name="using-blob-storage-in-the-fix-it-app"></a>Usando o armazenamento de BLOBs no aplicativo corrigir ti

O aplicativo corrigir ti permite que você carregue fotos.

![Criar uma tarefa corrigir ti](unstructured-blob-storage/_static/image2.png)

Quando você clica em **criar o Fixit**, o aplicativo carrega o arquivo de imagem especificado e o armazena no serviço BLOB.

### <a name="set-up-the-blob-container"></a>Configurar o contêiner de BLOBs

Para armazenar um arquivo no serviço BLOB, você precisa de um *contêiner* para armazená-lo. Um contêiner de serviço blob corresponde a uma pasta do sistema de arquivos. Os scripts de criação de ambiente que analisamos no [capítulo automatizar tudo](automate-everything.md) criam a conta de armazenamento, mas não criam um contêiner. Portanto, a finalidade do método de `CreateAndConfigure` da classe `PhotoService` é criar um contêiner, caso ele ainda não exista. Esse método é chamado a partir do método `Application_Start` no *global. asax*.

[!code-csharp[Main](unstructured-blob-storage/samples/sample2.cs)]

O nome da conta de armazenamento e a chave de acesso são armazenados na coleção de `appSettings` do arquivo *Web. config* , e o código no método `StorageUtils.StorageAccount` usa esses valores para criar uma cadeia de conexão e estabelecer uma conexão:

[!code-csharp[Main](unstructured-blob-storage/samples/sample3.cs)]

Em seguida, o método `CreateAndConfigureAsync` cria um objeto que representa o serviço BLOB e um objeto que representa um contêiner (pasta) chamado "imagens" no serviço blob:

[!code-csharp[Main](unstructured-blob-storage/samples/sample4.cs)]

Se um contêiner chamado "imagens" ainda não existir--que será verdadeiro na primeira vez em que você executar o aplicativo em uma nova conta de armazenamento, o código criará o contêiner e definirá permissões para torná-lo público. (Por padrão, novos contêineres de blob são privados e são acessíveis somente para usuários que têm permissão para acessar sua conta de armazenamento.)

[!code-csharp[Main](unstructured-blob-storage/samples/sample5.cs)]

### <a name="store-the-uploaded-photo-in-blob-storage"></a>Armazenar a foto carregada no armazenamento de BLOBs

Para carregar e salvar o arquivo de imagem, o aplicativo usa uma interface `IPhotoService` e uma implementação da interface na classe `PhotoService`. O arquivo *PhotoService.cs* contém todo o código no aplicativo Fix it que se comunica com o serviço BLOB.

O método do controlador MVC a seguir é chamado quando o usuário clica em **criar o Fixit**. Nesse código, `photoService` se refere a uma instância da classe `PhotoService` e `fixittask` se refere a uma instância da classe de entidade `FixItTask` que armazena dados para uma nova tarefa.

[!code-csharp[Main](unstructured-blob-storage/samples/sample6.cs?highlight=8)]

O método `UploadPhotoAsync` na classe `PhotoService` armazena o arquivo carregado no serviço BLOB e retorna uma URL que aponta para o novo BLOB.

[!code-csharp[Main](unstructured-blob-storage/samples/sample7.cs)]

Como no método `CreateAndConfigure`, o código se conecta à conta de armazenamento e cria um objeto que representa o contêiner de blob "images", exceto aqui, ele supõe que o contêiner já existe.

Em seguida, ele cria um identificador exclusivo para a imagem a ser carregada, concatenando um novo valor de GUID com a extensão de arquivo:

[!code-csharp[Main](unstructured-blob-storage/samples/sample8.cs)]

Em seguida, o código usa o objeto de contêiner de BLOB e o novo identificador exclusivo para criar um objeto de BLOB, define um atributo nesse objeto indicando que tipo de arquivo é e, em seguida, usa o objeto de BLOB para armazenar o arquivo no armazenamento de BLOBs.

[!code-csharp[Main](unstructured-blob-storage/samples/sample9.cs)]

Por fim, ele obtém uma URL que faz referência ao blob. Essa URL será armazenada no banco de dados e poderá ser usada nas páginas da Web Fix it para exibir a imagem carregada.

[!code-csharp[Main](unstructured-blob-storage/samples/sample10.cs)]

Essa URL é armazenada no banco de dados como uma das colunas da tabela FixItTask.

[!code-csharp[Main](unstructured-blob-storage/samples/sample11.cs?highlight=10)]

Com apenas a URL no banco de dados e as imagens no armazenamento de BLOBs, o aplicativo corrigir ti mantém o banco de dados pequeno, escalonável e barato, enquanto as imagens são armazenadas onde o armazenamento é barato e capaz de lidar com terabytes ou petabytes. Uma conta de armazenamento pode armazenar centenas de terabytes de correção de fotos de ti e você só paga pelo que usar. Portanto, você pode começar a pagar pequenas até 9 centavos pelo primeiro gigabyte e adicionar mais imagens para pennies por gigabyte adicional.

### <a name="display-the-uploaded-file"></a>Exibir o arquivo carregado

O aplicativo corrigir ti exibe o arquivo de imagem carregado quando exibe detalhes de uma tarefa.

![Corrigir detalhes da tarefa de ti com foto](unstructured-blob-storage/_static/image3.png)

Para exibir a imagem, tudo o que a exibição MVC precisa fazer é incluir o `PhotoUrl` valor no HTML enviado ao navegador. O servidor Web e o banco de dados não estão usando ciclos para exibir a imagem, eles estão servindo apenas alguns bytes para a URL da imagem. No código do Razor a seguir, `Model` se refere a uma instância da classe de entidade `FixItTask`.

[!code-cshtml[Main](unstructured-blob-storage/samples/sample12.cshtml?highlight=11)]

Se você olhar o HTML da página exibida, verá a URL apontando diretamente para a imagem no armazenamento de BLOBs, algo assim:

[!code-cshtml[Main](unstructured-blob-storage/samples/sample13.cshtml?highlight=11)]

## <a name="summary"></a>Resumo

Você viu como o aplicativo corrigir ti armazena imagens no serviço BLOB e apenas URLs de imagem no banco de dados SQL. Usar o serviço blob mantém o banco de dados SQL muito menor do que seria, o que torna possível escalar verticalmente para um número quase ilimitado de tarefas e pode ser feito sem escrever muito código.

Você pode ter centenas de terabytes em uma conta de armazenamento, e o custo de armazenamento é muito mais barato do que o armazenamento do banco de dados SQL, começando com cerca de 3 centavos por gigabyte por mês mais um pequeno encargo de transação. E tenha em mente que você não está pagando pela capacidade máxima, mas apenas pelo valor que realmente armazena, de modo que seu aplicativo esteja pronto para ser dimensionado, mas você não está pagando por toda essa capacidade extra.

No [próximo capítulo](design-to-survive-failures.md) , falaremos sobre a importância de tornar um aplicativo de nuvem capaz de lidar normalmente com falhas.

## <a name="resources"></a>Recursos

Para obter mais informações, consulte os seguintes recursos:

- [Uma introdução ao armazenamento de BLOBs do Azure](https://www.simple-talk.com/cloud/cloud-data/an-introduction-to-windows-azure-blob-storage-/). Blog de Mike madeira.
- [Como usar o serviço de armazenamento de BLOBs do Azure no .net](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs). Documentação oficial no site do MicrosoftAzure.com. Uma breve introdução ao armazenamento de BLOBs, seguida por exemplos de código que mostram como se conectar ao armazenamento de BLOBs, criar contêineres, carregar e baixar BLOBs, etc.
- [FailSafe: criando serviços de nuvem escalonáveis e resilientes](https://channel9.msdn.com/Series/FailSafe). Série de vídeos de nove partes por Ulrich Homann, Marc Mercuri e marcas Simms. Apresenta conceitos de alto nível e princípios arquitetônicos de uma maneira muito acessível e interessante, com histórias extraídas da experiência CAT (equipe de consultoria para clientes) da Microsoft com os clientes reais. Para obter uma discussão sobre o serviço de armazenamento do Azure e BLOBs, consulte Episódio 5 a partir de 35:13.
- [Práticas e padrões da Microsoft – diretrizes do Azure](https://msdn.microsoft.com/library/dn568099.aspx). Consulte padrão valet Key.

> [!div class="step-by-step"]
> [Anterior](data-partitioning-strategies.md)
> [Próximo](design-to-survive-failures.md)
