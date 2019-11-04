---
uid: web-api/overview/testing-and-debugging/troubleshooting-http-405-errors-after-publishing-web-api-applications
title: Solucionar problemas de aplicativos Web API2 que funcionam no Visual Studio e falham em um servidor IIS de produção
author: rmcmurray
description: Solucionar problemas de aplicativos Web API2 que funcionam no Visual Studio e falham em um servidor IIS de produção
ms.author: riande
ms.date: 01/23/2019
ms.assetid: 07ec7d37-023f-43ea-b471-60b08ce338f7
msc.legacyurl: /web-api/overview/testing-and-debugging/troubleshooting-http-405-errors-after-publishing-web-api-applications
msc.type: authoredcontent
ms.openlocfilehash: 1b47f1ade3619cfd010260352f6a96985ab3598b
ms.sourcegitcommit: 84b1681d4e6253e30468c8df8a09fe03beea9309
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/02/2019
ms.locfileid: "73445703"
---
# <a name="troubleshoot-web-api2-apps-that-work-in-visual-studio-and-fail-on-a-production-iis-server"></a>Solucionar problemas de aplicativos Web API2 que funcionam no Visual Studio e falham em um servidor IIS de produção

> Este documento explica como solucionar problemas de aplicativos Web API2 que são implantados em um servidor IIS de produção. Ele aborda erros comuns de HTTP 405 e 501.
> 
> ## <a name="software-used-in-this-tutorial"></a>Software usado neste tutorial
> 
> 
> - [Serviços de informações da Internet (IIS)](https://www.iis.net/) (versão 7 ou posterior)
> - [API Web](../../index.md) 

Os aplicativos de API Web normalmente usam vários verbos HTTP: GET, POST, PUT, DELETE e, às vezes, PATCH. Dito isso, os desenvolvedores podem se deparar com situações em que esses verbos são implementados por outro módulo do IIS em seu servidor IIS de produção, o que leva a uma situação em que um controlador de API da Web que funciona corretamente no Visual Studio ou em um servidor de desenvolvimento irá retornar um erro HTTP 405 quando ele for implantado em um servidor IIS de produção.

## <a name="what-causes-http-405-errors"></a>O que causa erros HTTP 405

A primeira etapa para aprender a solucionar erros de HTTP 405 é entender o que um erro HTTP 405 realmente significa. O documento principal de controle para HTTP é [RFC 2616](http://www.ietf.org/rfc/rfc2616.txt), que define o código de status http 405 como ***método não permitido***e descreve ainda mais esse código de status como uma situação em que &quot;o método especificado na linha de solicitação não é permitido para o recurso identificado pelo URI de solicitação.&quot; em outras palavras, o verbo HTTP não é permitido para a URL específica que um cliente HTTP solicitou.

Como uma breve revisão, aqui estão vários dos métodos de HTTP mais usados, conforme definido em RFC 2616, RFC 4918 e RFC 5789:

| Método HTTP | Descrição |
| --- | --- |
| **Obter** | Esse método é usado para recuperar dados de um URI e, provavelmente, o método HTTP mais usado. |
| **HEAD** | Esse método é muito parecido com o método GET, exceto que ele não recupera realmente os dados do URI de solicitação – ele simplesmente recupera o status HTTP. |
| **Postar** | Normalmente, esse método é usado para enviar novos dados para o URI; A POSTAgem é frequentemente usada para enviar dados de formulário. |
| **Posicione** | Esse método é normalmente usado para enviar dados brutos para o URI; PUT geralmente é usado para enviar dados JSON ou XML para aplicativos de API Web. |
| **DELETE** | Esse método é usado para remover dados de um URI. |
| **OPTIONS** | Esse método é normalmente usado para recuperar a lista de métodos HTTP com suporte para um URI. |
| **COPIAR MOVIMENTAÇÃO** | Esses dois métodos são usados com o WebDAV e sua finalidade é auto-explicativa. |
| **MKCOL** | Esse método é usado com o WebDAV e é usado para criar uma coleção (por exemplo, um diretório) no URI especificado. |
| **PROPPATCH PROPFIND** | Esses dois métodos são usados com o WebDAV e são usados para consultar ou definir propriedades para um URI. |
| **DESBLOQUEAR BLOQUEIO** | Esses dois métodos são usados com o WebDAV e são usados para bloquear/desbloquear o recurso identificado pelo URI de solicitação durante a criação. |
| **DISTRIBUÍDO** | Esse método é usado para modificar um recurso HTTP existente. |

Quando um desses métodos HTTP estiver configurado para uso no servidor, o servidor responderá com o status HTTP e outros dados que são apropriados para a solicitação. (Por exemplo, um método GET pode receber uma resposta HTTP 200 ***OK*** e um método Put pode receber uma resposta http 201 ***criada*** .)

Se o método HTTP não estiver configurado para uso no servidor, o servidor responderá com um erro HTTP 501 ***não implementado*** .

No entanto, quando um método HTTP é configurado para uso no servidor, mas ele foi desabilitado para um determinado URI, o servidor responderá com um erro de método HTTP 405 ***não permitido*** .

## <a name="example-http-405-error"></a>Erro HTTP 405 de exemplo

O exemplo de solicitação e resposta HTTP a seguir ilustram uma situação em que um cliente HTTP está tentando colocar o valor em um aplicativo de API da Web em um servidor Web, e o servidor retorna um erro HTTP que declara que o método PUT não é permitido:

Solicitação HTTP:

[!code-console[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample1.cmd)]

Resposta HTTP:

[!code-console[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample2.cmd)]

Neste exemplo, o cliente HTTP enviou uma solicitação JSON válida para a URL de um aplicativo de API Web em um servidor Web, mas o servidor retornou uma mensagem de erro HTTP 405 que indica que o método PUT não foi permitido para a URL. Por outro lado, se o URI de solicitação não coincidir com uma rota para o aplicativo de API Web, o servidor retornará um erro HTTP 404 ***não encontrado*** .

## <a name="resolve-http-405-errors"></a>Resolver erros HTTP 405

Há várias razões pelas quais um verbo HTTP específico pode não ser permitido, mas há um cenário primário que é a causa principal desse erro no IIS: vários manipuladores são definidos para o mesmo verbo/método, e um dos manipuladores está bloqueando o manipulador esperado de processando a solicitação. Por meio de explicação, o IIS processa os manipuladores do primeiro até o último com base nas entradas do manipulador de pedidos nos arquivos *ApplicationHost. config* e *Web. config* , em que a primeira combinação correspondente de caminho, verbo, recurso etc., será usada para manipular a solicitação.

O exemplo a seguir é um trecho de um arquivo *ApplicationHost. config* para um servidor IIS que estava retornando um erro http 405 ao usar o método Put para enviar dados a um aplicativo de API da Web. Neste trecho, vários manipuladores HTTP são definidos, e cada manipulador tem um conjunto diferente de métodos HTTP para o qual está configurado-a última entrada na lista é o manipulador de conteúdo estático, que é o manipulador padrão que é usado depois que os outros manipuladores tinham um chanc e para examinar a solicitação:

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample3.xml)]

No exemplo anterior, o manipulador WebDAV e o manipulador de URL sem extensão para ASP.NET (que é usado para API Web) são claramente definidos para listas separadas de métodos HTTP. Observe que o manipulador de DLL ISAPI está configurado para todos os métodos HTTP, embora essa configuração não cause, necessariamente, um erro. No entanto, as definições de configuração como essa precisam ser consideradas ao solucionar erros de HTTP 405.

No exemplo anterior, o manipulador de DLL ISAPI não era o problema; na verdade, o problema não foi definido no arquivo *ApplicationHost. config* para o servidor IIS-o problema foi causado por uma entrada que foi feita no arquivo *Web. config* quando o aplicativo da API Web foi criado no Visual Studio. O trecho a seguir do arquivo *Web. config* do aplicativo mostra o local do problema:

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample4.xml)]

Neste trecho, o manipulador de URL sem extensão para ASP.NET é redefinido para incluir métodos HTTP adicionais que serão usados com o aplicativo de API da Web. No entanto, como um conjunto semelhante de métodos HTTP é definido para o manipulador WebDAV, ocorre um conflito. Nesse caso específico, o manipulador WebDAV é definido e carregado pelo IIS, embora o WebDAV esteja desabilitado para o site que inclui o aplicativo de API da Web. Durante o processamento de uma solicitação HTTP PUT, o IIS chama o módulo WebDAV, já que ele é definido para o verbo PUT. Quando o módulo WebDAV é chamado, ele verifica sua configuração e vê que está desabilitado, portanto, retornará um erro HTTP 405 ***sem permissão*** para qualquer solicitação que se assemelha a uma solicitação WebDAV. Para resolver esse problema, você deve remover o WebDAV da lista de módulos HTTP para o site em que seu aplicativo de API Web é definido. O exemplo a seguir mostra como isso pode ser:

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample5.xml)]

Esse cenário geralmente é encontrado depois que um aplicativo é publicado de um ambiente de desenvolvimento para um ambiente de produção do IIS, e isso ocorre porque a lista de manipuladores/módulos é diferente entre seus ambientes de desenvolvimento e produção. Por exemplo, se você estiver usando o Visual Studio 2012 ou posterior para desenvolver um aplicativo de API Web, IIS Express será o servidor Web padrão para teste. Esse servidor Web de desenvolvimento é uma versão reduzida da funcionalidade completa do IIS que é fornecida em um produto de servidor, e esse servidor Web de desenvolvimento contém algumas alterações que foram adicionadas para cenários de desenvolvimento. Por exemplo, o módulo WebDAV geralmente é instalado em um servidor Web de produção que está executando a versão completa do IIS, embora talvez não esteja em uso. A versão de desenvolvimento do IIS, (IIS Express), instala o módulo WebDAV, mas as entradas do módulo WebDAV são intencionalmente comentadas, portanto, o módulo WebDAV nunca é carregado no IIS Express, a menos que você altere especificamente sua configuração de IIS Express configurações para adicionar a funcionalidade WebDAV à sua instalação do IIS Express. Como resultado, seu aplicativo Web pode funcionar corretamente no seu computador de desenvolvimento, mas você pode encontrar erros HTTP 405 ao publicar seu aplicativo de API Web no servidor Web do IIS de produção.

## <a name="http-501-errors"></a>Erros HTTP 501

* Indica que a funcionalidade específica não foi implementada no servidor.
* Normalmente significa que não há nenhum manipulador definido nas configurações do IIS que corresponda à solicitação HTTP:
  * Provavelmente indica que algo não foi instalado corretamente no IIS ou
  * Algo modificou as configurações do IIS para que não haja manipuladores definidos que ofereçam suporte ao método HTTP específico.

Para resolver esse problema, você precisará reinstalar qualquer aplicativo que esteja tentando usar um método HTTP para o qual não tenha nenhuma definição de módulo ou manipulador correspondente.

## <a name="summary"></a>Resumo

Erros HTTP 405 são causados quando um método HTTP não é permitido por um servidor Web para uma URL solicitada. Essa condição geralmente é vista quando um manipulador específico foi definido para um verbo específico e esse manipulador está substituindo o manipulador que você espera processar a solicitação.
