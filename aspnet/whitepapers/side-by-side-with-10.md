---
uid: whitepapers/side-by-side-with-10
title: ASP.NET a execução lado a lado de .NET Framework 1,0 e 1,1 | Microsoft Docs
author: rick-anderson
description: Este white paper descreve como instalar o .NET 1,0 e o .NET 1,1 em seu computador, permitindo que um aplicativo Web ASP.NET seja executado em qualquer uma das versões do fram...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: bdea2003-e964-4db5-9092-d56cc7560616
msc.legacyurl: /whitepapers/side-by-side-with-10
msc.type: content
ms.openlocfilehash: c123545099013af71569bce4707f2b3eb732c344
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78632968"
---
# <a name="aspnet-side-by-side-execution-of-net-framework-10-and-11"></a>Execução lado a lado do ASP.NET no .NET Framework 1.0 e 1.1

> Este white paper descreve como instalar o .NET 1,0 e o .NET 1,1 em seu computador, permitindo que um aplicativo Web ASP.NET seja executado em qualquer uma das versões do Framework.
> 
> Aplica-se a ASP.NET 1,0 e ASP.NET 1,1.

No ASP.NET, os aplicativos são considerados em execução lado a lado quando são instalados no mesmo computador, mas usam versões diferentes do .NET Framework. O tópico a seguir descreve como configurar aplicativos ASP.NET para execução lado a lado e fornece etapas detalhadas para:

- [Manter o mapeamento do aplicativo Web para .NET Framework versão 1,0 durante a instalação](#1)
- [Mapear um aplicativo Web para uma versão específica do .NET Framework](#2)
- [Localizar a versão do .NET Framework que um site está usando](#3)

Tradicionalmente, quando um componente ou aplicativo é atualizado em um computador, a versão mais antiga é removida e substituída pela versão mais recente. Se a nova versão não for compatível com a versão anterior, isso geralmente interromperá outros aplicativos que usam o componente ou aplicativo. O .NET Framework fornece suporte para a execução lado a lado, o que permite que várias versões de um assembly ou aplicativo sejam instaladas no mesmo computador ao mesmo tempo. Como várias versões podem ser instaladas simultaneamente, os aplicativos gerenciados podem selecionar qual versão usar sem afetar os aplicativos que usam uma versão diferente.

Por padrão, durante a instalação da versão 1,1 do .NET Framework, todos os aplicativos ASP.NET existentes são automaticamente reconfigurados para usar a versão mais recente do .NET Framework. Se você não quiser que seus aplicativos ASP.NET sejam padronizados para .NET Framework 1,1, clique [aqui](#1) para saber como evitar isso durante a instalação.

Se você atualizar seu servidor Web para .NET Framework 1,1 e desejar que um ou mais aplicativos Web sejam executados .NET Framework 1,0, você precisará atualizar o mapa de script do Serviços de Informações da Internet (IIS). O mapeamento de script é o mecanismo para mapear a extensão de arquivo. aspx para um aplicativo Web específico para uma versão do .NET Framework. Clique [aqui](#2) para saber como mapear um aplicativo Web para uma versão específica do .NET Framework.

Você pode usar o Gerenciador de informações da Internet ou a ferramenta de registro do ASP.NET IIS (ASPNET\_regiis. exe) para descobrir qual versão do .NET Framework está executando um aplicativo Web específico. Clique [aqui](#3) para saber como localizar a versão do .NET Framework que um site da Web está usando.

Uma consideração de importação ao migrar para o .NET Framework 1,1 é que cada versão do .NET Framework usa seu próprio arquivo Machine. config. Como resultado, se um administrador da Web tiver feito alterações no arquivo Machine. config, essas alterações deverão ser migradas para o arquivo Machine. config do .NET Framework 1,1.

<a id="1"></a>

## <a name="maintaining-your-web-applications-mapping-to-net-framework-10-during-installation"></a>Manter o mapeamento do aplicativo Web para .NET Framework 1,0 durante a instalação

Por padrão, todos os aplicativos ASP.NET existentes são reconfigurados automaticamente durante a instalação para usar a versão mais recente do .NET Framework. Usando a versão mais recente do .NET Framework, os aplicativos podem aproveitar ao máximo os aprimoramentos e os novos recursos incluídos na nova versão. Ao mesmo tempo, o administrador da Web, que pode querer ter um controle granular sobre quais aplicativos são atualizados, pode impedir o remapeamento automático de todos os aplicativos ASP.NET existentes durante a instalação do .NET Framework.

Para evitar o remapeamento automático de todo o aplicativo ASP.NET para a versão mais recente do .NET Framework, o administrador da Web pode usar a opção de linha de comando/noaspupgrade com o programa de instalação do Dotnetfx. exe.

**Para evitar o remapeamento total do aplicativo ASP.NET para uma versão mais recente**

1. Vá para **Iniciar**.
2. Clique em **executar**.
3. Digite **cmd**.
4. Clique em **OK**.  
  
    ![](side-by-side-with-10/_static/image1.gif)
5. No prompt de comando, digite a seguinte linha para iniciar a instalação do .NET Framework: **Dotnetfx. exe/c: "instalar o/noaspupgrade?** .  
  
    ![](side-by-side-with-10/_static/image2.gif)
6. Clique em **Sim** na instalação do Microsoft .NET Framework 1,1. Isso iniciará o processo de instalação do .NET Framework 1,1.  
  
    ![](side-by-side-with-10/_static/image3.gif)

<a id="2"></a>

## <a name="map-a-web-application-to-a-specific-version-of-the-net-framework"></a>Mapear um aplicativo Web para uma versão específica do .NET Framework

Cada versão do .NET Framework inclui uma versão da ferramenta de registro do IIS do ASP.NET (ASPNET\_regiis. exe). Essa ferramenta permite que os administradores especifiquem que um aplicativo Web seja executado em uma versão específica do .NET Framework. Isso é conhecido como mapeamento de um aplicativo Web para uma versão do .NET Framework. Os administradores devem selecionar o ASPNET\_regiis. exe que corresponde à versão do .NET Framework que será associado ao aplicativo Web. Por exemplo, um administrador que deseja especificar que um site usa .NET Framework 1,1 deve usar o ASPNET\_regiis. exe que vem com .NET Framework 1,1.

O ASPNET\_regiis. exe para a versão 1,0 está localizado em:

- C:\WINDOWS\Microsoft.NET\Framework\\**v 1.0.3705**\ASPNET\_regiis

O ASPNET\_regiis. exe para a versão 1, 1 está localizado em:

- C:\WINDOWS\Microsoft.NET\Framework\\**v 1.1.4322**\ASPNET\_regiis

O ASPNET\_regiis. exe fornece duas opções para o mapeamento de script de um aplicativo Web:

- **-s** define o mapa de script no caminho e em seus diretórios filho.
- **-SN** define o mapa de script somente no caminho.

O caminho define o caminho de metadados do IIS do aplicativo Web, que é definido na forma de W3SVC/ROOT/{WebSiteNumber}/{nome do aplicativo\_}. Por exemplo, para um aplicativo Web chamado portal localizado no site da Web padrão, o caminho da metabase é W3SVC/1/ROOT/Portal.

![](side-by-side-with-10/_static/image4.gif)

Observação Você também pode usar uma ferramenta chamada editor de metabase para obter o caminho da metabase. Você pode baixar essa ferramenta no site do Suporte da Microsoft em [https://support.microsoft.com/default.aspx?scid=kb; en-US; 232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)

- Execute ASPNET\_Regiss. exe-s W3SVC/1/ROOT/portal para atualizar o mapa de script do IIS do portal e seu subaplicativo.  
  
    ![](side-by-side-with-10/_static/image5.gif)

- Execute ASPNET\_regiry. exe-SN W3SVC/1/ROOT/portal para atualizar o mapa de script do IIS do portal, sem afetar os aplicativos nos subdiretórios do portal? s.  
  
    ![](side-by-side-with-10/_static/image6.gif)

<a id="3"></a>

## <a name="find-the-net-framework-version-that-a-web-application-is-using"></a>Localizar a versão de .NET Framework que um aplicativo Web está usando

Um administrador pode usar o Service Manager da Internet para descobrir qual versão do .NET Framework executa um site. Versões diferentes do sistema operacional iniciam a Internet Service Manager de maneira diferente. Para iniciar o Service Manager, siga as etapas listadas abaixo.

**Para iniciar o Internet Service Manager**

1. Vá para **Iniciar**.
2. Clique em **executar**.
3. Digite **inetmgr**.  
  
    ![](side-by-side-with-10/_static/image7.gif)
4. Na Service Manager da Internet, selecione o aplicativo Web cuja versão do .NET Framework você deseja saber.  
  
    ![](side-by-side-with-10/_static/image8.gif)
5. Clique com o botão direito do mouse no aplicativo Web e clique em **Propriedades.**  
  
    ![](side-by-side-with-10/_static/image9.gif)
6. Na janela de propriedades, selecione **configuração.**  
  
    ![](side-by-side-with-10/_static/image10.gif)
7. Na tabela de mapeamento de aplicativos, selecione **. aspx**e clique em **Editar**.  
  
    ![](side-by-side-with-10/_static/image11.gif)
8. Na caixa de texto **executável** , examine o diretório da versão rolando. Se o diretório da versão for v. 1.1.4322, o aplicativo será mapeado para .NET Framework 1,1. Por outro lado, se o diretório da versão for v 1.0.3705, o aplicativo será mapeado para .NET Framework 1,0.  
  
    ![](side-by-side-with-10/_static/image12.gif)
