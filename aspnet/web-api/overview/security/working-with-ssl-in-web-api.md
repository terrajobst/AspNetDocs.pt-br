---
uid: web-api/overview/security/working-with-ssl-in-web-api
title: Trabalhando com SSL na API Web | Microsoft Docs
author: MikeWasson
description: Mostra como usar SSL com ASP.NET Web API, incluindo o uso de certificados de cliente SSL.
ms.author: riande
ms.date: 02/22/2019
ms.assetid: 97f6164f-59cf-45c0-b820-e4aa29b45396
msc.legacyurl: /web-api/overview/security/working-with-ssl-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 31589b3713b1f1a9b98d12906bfef81f8bf5e3f9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598633"
---
# <a name="working-with-ssl-in-web-api"></a>Trabalhar com SSL na API da Web

por [Mike Wasson](https://github.com/MikeWasson)

Vários esquemas de autenticação comuns não são seguros em relação a HTTP simples. Em particular, a autenticação Básica e a autenticação de formulários enviam credenciais sem criptografia. Para ser seguro, esses esquemas de autenticação *devem* usar SSL. Além disso, os certificados de cliente SSL podem ser usados para autenticar clientes.

## <a name="enabling-ssl-on-the-server"></a>Habilitando SSL no servidor

Para configurar o SSL no IIS 7 ou posterior:

- Crie ou obtenha um certificado. Para teste, você pode criar um certificado autoassinado.
- Adicione uma associação HTTPS.

Para obter detalhes, consulte [como configurar o SSL no IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).

Para testes locais, você pode habilitar o SSL no IIS Express do Visual Studio. Na janela Propriedades, defina **SSL habilitado** como **verdadeiro**. Observe o valor da **URL SSL**; Use esta URL para testar conexões HTTPS.

![](working-with-ssl-in-web-api/_static/image1.png)

### <a name="enforcing-ssl-in-a-web-api-controller"></a>Impondo SSL em um controlador de API da Web

Se você tiver uma associação HTTPS e HTTP, os clientes ainda poderão usar HTTP para acessar o site. Você pode permitir que alguns recursos estejam disponíveis por meio de HTTP, enquanto outros recursos exigem SSL. Nesse caso, use um filtro de ação para exigir o SSL para os recursos protegidos. O código abaixo mostra um filtro de autenticação de API da Web que verifica o SSL:

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample1.cs)]

Adicione esse filtro às ações de API da Web que exigem SSL:

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample2.cs)]

## <a name="ssl-client-certificates"></a>Certificados de cliente SSL

O SSL fornece autenticação usando certificados de infraestrutura de chave pública. O servidor deve fornecer um certificado que autentique o servidor para o cliente. É menos comum que o cliente forneça um certificado para o servidor, mas essa é uma opção para autenticar clientes. Para usar certificados de cliente com SSL, você precisa de uma maneira de distribuir certificados assinados para seus usuários. Para muitos tipos de aplicativos, essa não será uma boa experiência do usuário, mas em alguns ambientes (por exemplo, empresa) pode ser viável.

| Vantagens | Desvantagens |
| --- | --- |
| -As credenciais do certificado são mais fortes do que o nome de usuário/senha. -O SSL fornece um canal seguro completo, com autenticação, integridade de mensagem e criptografia de mensagem. | -Você deve obter e gerenciar certificados PKI. -A plataforma do cliente deve dar suporte a certificados de cliente SSL. |

Para configurar o IIS para aceitar certificados de cliente, abra o Gerenciador do IIS e execute as seguintes etapas:

1. Clique no nó do site no modo de exibição de árvore.
2. Clique duas vezes no recurso **configurações de SSL** no painel central.
3. Em **certificados de cliente**, selecione uma destas opções: 

    - **Aceitar**: o IIS aceitará um certificado do cliente, mas não precisa de um.
    - **Exigir**: exigir um certificado de cliente. (Para habilitar essa opção, você também deve selecionar "exigir SSL")

Você também pode definir essas opções no arquivo ApplicationHost. config:

[!code-xml[Main](working-with-ssl-in-web-api/samples/sample3.xml)]

O sinalizador **SslNegotiateCert** significa que o IIS aceitará um certificado do cliente, mas não requer um (equivalente à opção "Accept" no Gerenciador do IIS). Para exigir um certificado, defina o sinalizador **SslRequireCert** . Para teste, você também pode definir essas opções no IIS Express, no ApplicationHost local. Arquivo de configuração, localizado em "Documents\IISExpress\config".

### <a name="creating-a-client-certificate-for-testing"></a>Criando um certificado de cliente para teste

Para fins de teste, você pode usar [MakeCert. exe](/windows/desktop/SecCrypto/makecert) para criar um certificado de cliente. Primeiro, crie uma autoridade raiz de teste:

[!code-console[Main](working-with-ssl-in-web-api/samples/sample4.cmd)]

O MakeCert solicitará que você insira uma senha para a chave privada.

Em seguida, adicione o certificado ao repositório "autoridades de certificação raiz confiáveis" do servidor de teste, da seguinte maneira:

1. Abra o MMC.
2. Em **arquivo**, selecione **Adicionar/remover snap-in**.
3. Selecione **conta de computador**.
4. Selecione **computador local** e conclua o assistente.
5. No painel de navegação, expanda o nó "autoridades de certificação raiz confiáveis".
6. No menu **ação** , aponte para **todas as tarefas**e clique em **importar** para iniciar o assistente para importação de certificados.
7. Navegue até o arquivo de certificado, TempCA. cer.
8. Clique em **abrir**e em **Avançar** e conclua o assistente. (Será solicitado que você insira novamente a senha.)

Agora, crie um certificado de cliente que é assinado pelo primeiro certificado:

[!code-console[Main](working-with-ssl-in-web-api/samples/sample5.cmd)]

### <a name="using-client-certificates-in-web-api"></a>Usando certificados de cliente na API Web

No lado do servidor, você pode obter o certificado do cliente chamando [GetClientCertificate](https://msdn.microsoft.com/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx) na mensagem de solicitação. O método retornará NULL se não houver nenhum certificado de cliente. Caso contrário, ele retornará uma instância de **X509Certificate2** . Use esse objeto para obter informações do certificado, como o emissor e o assunto. Em seguida, você pode usar essas informações para autenticação e/ou autorização.

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample6.cs)]
