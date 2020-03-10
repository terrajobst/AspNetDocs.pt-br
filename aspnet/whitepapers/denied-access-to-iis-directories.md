---
uid: whitepapers/denied-access-to-iis-directories
title: ASP.NET negou acesso aos diretórios do IIS | Microsoft Docs
author: rick-anderson
description: Este white paper descreve o que você deve fazer se uma solicitação ao aplicativo ASP.NET retornar o erro "acesso negado ao diretório DirectoryName. Falha em s...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: 3cb27b8a-354f-4332-bfe0-232b13bbf8aa
msc.legacyurl: /whitepapers/denied-access-to-iis-directories
msc.type: content
ms.openlocfilehash: a3a53aa88abbe1bcaaea7d691406800c8f9b988b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78638498"
---
# <a name="aspnet-denied-access-to-iis-directories"></a>Acesso negado do ASP.NET a diretórios do IIS

> Este white paper descreve o que você deve fazer se uma solicitação ao aplicativo ASP.NET retornar o erro "acesso negado ao diretório *DirectoryName* . Falha ao iniciar o monitoramento de alterações de diretório. "
> 
> Aplica-se a ASP.NET 1,0 e ASP.NET 1,1.

O ASP.NET v1 RTM agora é executado usando uma conta do Windows menos privilegiada – registrada como a conta "ASPNET" em um computador local.

Em alguns sistemas bloqueados, essa conta pode não, por padrão, ter acesso de segurança de leitura aos diretórios de conteúdo de um site, ao diretório raiz do aplicativo ou ao diretório raiz do site. Nesse caso, você receberá o seguinte erro ao solicitar páginas de um determinado aplicativo Web:

![](denied-access-to-iis-directories/_static/image1.jpg)

Para corrigir isso, você precisará alterar as permissões de segurança nos diretórios apropriados.

Especificamente, o ASP.NET requer acesso de leitura, execução e lista para a conta ASPNET para a raiz do site (por exemplo: c:\inetpub\wwwroot ou qualquer diretório de site alternativo que você possa ter configurado no IIS), o diretório de conteúdo e o diretório raiz do aplicativo para monitorar as alterações do arquivo de configuração. A raiz do aplicativo corresponde ao caminho da pasta associado ao diretório virtual do aplicativo na ferramenta de administração do IIS (inetmgr).

Por exemplo, considere a seguinte hierarquia de aplicativo na pasta wwwroot.

`C:\inetpub\wwwroot\myapp\default.aspx`

Para este exemplo, a conta ASPNET precisa das permissões de leitura definidas acima para o conteúdo no diretório MyApp e wwwroot. Uma única ACL herdada na pasta raiz também pode ser usada opcionalmente para ambos os diretórios se eles estiverem aninhados.

Para adicionar permissões a um diretório, execute as seguintes etapas:

- Usando o Windows Explorer, navegue até o diretório
- Clique com o botão direito do mouse na pasta do diretório e escolha "Propriedades"
- Navegue até a guia "segurança" na caixa de diálogo de propriedade
- Clique no botão "Adicionar" e insira o nome do computador seguido pelo nome da conta ASPNET. Por exemplo, em um computador chamado "webdev", você deve digitar webdev\ASPNET e clicar em "OK".
- Verifique se a conta ASPNET tem as caixas de seleção "ler &amp; executar", "listar conteúdo da pasta" e "ler" marcada.
- Pressione OK para ignorar a caixa de diálogo e salvar as alterações.

![](denied-access-to-iis-directories/_static/image2.jpg)

Se desejar, essas alterações podem ser automatizadas usando scripts ou a ferramenta "Cacls. exe" fornecida com o Windows. Para obter mais informações sobre a conta ASPNET, consulte o [documento de perguntas frequentes](https://go.microsoft.com/fwlink/?LinkId=5828).

Se um determinado aplicativo Web depender de permissões de gravação ou modificação em uma pasta ou arquivo específico, isso poderá ser concedido seguindo o mesmo procedimento e marcando as caixas de seleção "gravar" e/ou "Modificar".

Em computadores que permitem que todos ou o grupo usuários tenham acesso de leitura a esses diretórios (que é a configuração padrão), nenhum problema será encontrado e as etapas acima não serão necessárias.
