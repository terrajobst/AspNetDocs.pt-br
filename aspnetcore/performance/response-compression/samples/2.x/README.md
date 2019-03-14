---
ms.openlocfilehash: 976cc58dfcd9bba0b88ddd5d0d886cbb99b418ae
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57026313"
---
# <a name="response-compression-sample-application-aspnet-core-2x"></a><span data-ttu-id="d3201-101">Aplicativo de exemplo de compactação de resposta (ASP.NET Core 2.x)</span><span class="sxs-lookup"><span data-stu-id="d3201-101">Response compression sample application (ASP.NET Core 2.x)</span></span>

<span data-ttu-id="d3201-102">Este exemplo ilustra o uso do ASP.NET Core 2.x Middleware de compactação de resposta para compactar respostas HTTP.</span><span class="sxs-lookup"><span data-stu-id="d3201-102">This sample illustrates the use of ASP.NET Core 2.x Response Compression Middleware to compress HTTP responses.</span></span> <span data-ttu-id="d3201-103">O exemplo demonstra como provedores de compactação personalizado para respostas de texto e imagem, Brotli e Gzip e mostra como adicionar um tipo MIME para compactação.</span><span class="sxs-lookup"><span data-stu-id="d3201-103">The sample demonstrates Gzip, Brotli, and custom compression providers for text and image responses and shows how to add a MIME type for compression.</span></span> <span data-ttu-id="d3201-104">Para o exemplo do ASP.NET Core 1.x, consulte [aplicativo de exemplo de compactação de resposta (ASP.NET Core 1.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples/1.x).</span><span class="sxs-lookup"><span data-stu-id="d3201-104">For the ASP.NET Core 1.x sample, see [Response compression sample application (ASP.NET Core 1.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples/1.x).</span></span>

## <a name="examples-in-this-sample"></a><span data-ttu-id="d3201-105">Exemplos desta amostra</span><span class="sxs-lookup"><span data-stu-id="d3201-105">Examples in this sample</span></span>

* `BrotliCompressionProvider`
  * `text/plain`
    * <span data-ttu-id="d3201-106">**/** -Lorem Ipsum arquivo resposta de texto em bytes 2,044 que compacta a ~ 979 bytes.</span><span class="sxs-lookup"><span data-stu-id="d3201-106">**/** - Lorem Ipsum text file response at 2,044 bytes that compresses to ~979 bytes.</span></span>
    * <span data-ttu-id="d3201-107">**/testfile1kb.txt** -resposta de arquivo de texto em bytes 1,033 que compacta a ~ 36 bytes.</span><span class="sxs-lookup"><span data-stu-id="d3201-107">**/testfile1kb.txt** - Text file response at 1,033 bytes that compresses to ~36 bytes.</span></span>
    * <span data-ttu-id="d3201-108">**/ surjam** -resposta emitido como caracteres únicos em intervalos de 1 segundo.</span><span class="sxs-lookup"><span data-stu-id="d3201-108">**/trickle** - Response issued as single characters at 1 second intervals.</span></span>
* `GzipCompressionProvider`
  * `text/plain`
    * <span data-ttu-id="d3201-109">**/** -Lorem Ipsum arquivo resposta de texto em bytes 2,044 que compacta a ~ 927 bytes.</span><span class="sxs-lookup"><span data-stu-id="d3201-109">**/** - Lorem Ipsum text file response at 2,044 bytes that compresses to ~927 bytes.</span></span>
    * <span data-ttu-id="d3201-110">**/testfile1kb.txt** -resposta de arquivo de texto em bytes 1,033 que compacta a ~ 47 bytes.</span><span class="sxs-lookup"><span data-stu-id="d3201-110">**/testfile1kb.txt** - Text file response at 1,033 bytes that compresses to ~47 bytes.</span></span>
    * <span data-ttu-id="d3201-111">**/ surjam** -resposta emitido como caracteres únicos em intervalos de 1 segundo.</span><span class="sxs-lookup"><span data-stu-id="d3201-111">**/trickle** - Response issued as single characters at 1 second intervals.</span></span>
  * `image/svg+xml`
    * <span data-ttu-id="d3201-112">**/banner.SVG** -resposta de imagem de um Scalable Vector Graphics (SVG) em bytes 9,707 que compacta a ~ 4,459 bytes.</span><span class="sxs-lookup"><span data-stu-id="d3201-112">**/banner.svg** - A Scalable Vector Graphics (SVG) image response at 9,707 bytes that compresses to ~4,459 bytes.</span></span>
* `CustomCompressionProvider`<br><span data-ttu-id="d3201-113">Mostra como implementar um provedor de compactação personalizado para uso com o middleware.</span><span class="sxs-lookup"><span data-stu-id="d3201-113">Shows how to implement a custom compression provider for use with the middleware.</span></span>

<span data-ttu-id="d3201-114">Quando a solicitação inclui o `Accept-Encoding` compactação de cabeçalho e a resposta for bem-sucedida, o middleware adiciona automaticamente um `Vary: Accept-Encoding` cabeçalho à resposta.</span><span class="sxs-lookup"><span data-stu-id="d3201-114">When the request includes the `Accept-Encoding` header and response compression is successful, the middleware automatically adds a `Vary: Accept-Encoding` header to the response.</span></span> <span data-ttu-id="d3201-115">O `Vary` cabeçalho instrui caches manter várias cópias da resposta com base nos valores alternativos de `Accept-Encoding`, portanto, ambos um compactado (Gzip ou Brotli) e versão não compactada são armazenados em caches de sistemas que podem aceitam o compactado ou descompactada resposta.</span><span class="sxs-lookup"><span data-stu-id="d3201-115">The `Vary` header instructs caches to maintain multiple copies of the response based on alternative values of `Accept-Encoding`, so both a compressed (Gzip or Brotli) and uncompressed version are stored in caches for systems that can either accept the compressed or the uncompressed response.</span></span>

## <a name="use-the-sample"></a><span data-ttu-id="d3201-116">Use o exemplo</span><span class="sxs-lookup"><span data-stu-id="d3201-116">Use the sample</span></span>

1. <span data-ttu-id="d3201-117">Fazer uma solicitação usando [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), ou [Postman](https://www.getpostman.com/) para o aplicativo sem um `Accept-Encoding` cabeçalho e observe a carga de resposta, o tamanho da resposta, e cabeçalhos de resposta.</span><span class="sxs-lookup"><span data-stu-id="d3201-117">Make a request using [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), or [Postman](https://www.getpostman.com/) to the application without an `Accept-Encoding` header and note the response payload, response size, and response headers.</span></span>
1. <span data-ttu-id="d3201-118">Adicionar um `Accept-Encoding: br` ou `Accept-Encoding: gzip` cabeçalho e observe o tamanho da resposta compactada e cabeçalhos de resposta.</span><span class="sxs-lookup"><span data-stu-id="d3201-118">Add an `Accept-Encoding: br` or `Accept-Encoding: gzip` header and note the compressed response size and response headers.</span></span> <span data-ttu-id="d3201-119">Descarta o tamanho da resposta e o `Content-Encoding` cabeçalho de resposta é incluído pelo middleware que indica que a compactação com qualquer um dos Gzip ou Brotli ocorreu.</span><span class="sxs-lookup"><span data-stu-id="d3201-119">The response size drops, and the `Content-Encoding` response header is included by the middleware indicating that compression with either Gzip or Brotli occurred.</span></span> <span data-ttu-id="d3201-120">Quando você examinar o corpo da resposta para o Lorem Ipsum ou **testfile1kb.txt** resposta, você verá que o texto é compactada e não pode ser lido.</span><span class="sxs-lookup"><span data-stu-id="d3201-120">When you look at the response body for the Lorem Ipsum or **testfile1kb.txt** response, you see that the text is compressed and unreadable.</span></span>
1. <span data-ttu-id="d3201-121">Adicionar um `Accept-Encoding: mycustomcompression` cabeçalho e observe os cabeçalhos de resposta.</span><span class="sxs-lookup"><span data-stu-id="d3201-121">Add an `Accept-Encoding: mycustomcompression` header and note the response headers.</span></span> <span data-ttu-id="d3201-122">O `CustomCompressionProvider` é uma implementação vazia que realmente não compactar a resposta, mas você pode criar um wrapper de fluxo de compactação personalizado para o `CreateStream()` método.</span><span class="sxs-lookup"><span data-stu-id="d3201-122">The `CustomCompressionProvider` is an empty implementation that doesn't actually compress the response, but you can create a custom compression stream wrapper for the `CreateStream()` method.</span></span>