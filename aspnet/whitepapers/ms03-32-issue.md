---
uid: whitepapers/ms03-32-issue
title: Correção do erro ' servidor indisponível ' após aplicar a atualização de segurança para o IE | Microsoft Docs
author: rick-anderson
description: Este documento descreve o patch que corrige um problema com a atualização de segurança do MS03-32 para o Internet Explorer que afeta os aplicativos ASP.NET 1,0 em execução no Wi...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: 1365eebb-bdf7-4a05-8d18-7f200531be55
msc.legacyurl: /whitepapers/ms03-32-issue
msc.type: content
ms.openlocfilehash: e0b6776cbfe22e341ac7105f03daac5074b480fc
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78574182"
---
# <a name="fix-for-server-application-unavailable-error-after-applying-security-update-for-ie"></a>Correção do erro "Aplicativo para servidores não disponível" depois de aplicar a atualização de segurança para o IE

> Este documento descreve o patch que corrige um problema com a atualização de segurança do MS03-32 para o Internet Explorer que afeta os aplicativos ASP.NET 1,0 em execução no Windows XP Professional.
> 
> Aplica-se ao ASP.NET 1,0 e ao Windows XP Professional.

A Microsoft identificou um problema com a atualização de segurança MS03-32 para o patch de segurança do Internet Explorer e o ASP.NET 1,0 em execução no Windows XP. Esse patch pode ser instalado manualmente ou obtendo atualizações críticas recentes do site Windows Update.

O sintoma desse problema é que depois de instalar o patch em um computador com Windows XP, todas as solicitações para aplicativos ASP.NET em execução no servidor Web local do IIS 5,1 resultam em uma mensagem de erro informando "aplicativo do servidor indisponível". As solicitações para servidores Web remotos não são afetadas.

Esse problema afeta apenas instalações que executam o ASP.NET 1,0 no Windows XP. Ele não afeta os computadores que executam o Windows 2000 ou o Windows Server 2003. Ele também não afeta os computadores que executam o Windows XP com o ASP.NET 1,1 instalado.

Observe que esse problema **não é** um bug de segurança com ASP.net. Ele **não abre nem permite** ataques mal-intencionados em um aplicativo ou servidor ASP.net. Em vez disso, é puramente um bug funcional causado pelo próprio patch.

Estamos trabalhando duro em uma solução permanente para esse problema. Enquanto isso, você pode executar o seguinte arquivo em lotes como uma solução alternativa para o problema. O arquivo em lotes faz o seguinte:

1. Interrompe os serviços de estado do IIS e ASP.NET
2. Exclui e recria a conta ASPNET com uma senha temporária conhecida
3. Usa o comando `runas` do Windows para iniciar um executável que cria um perfil de usuário ASPNET
4. Registra novamente ASP.NET. Isso cria uma nova senha aleatória para a conta e aplica as configurações de controle de acesso ASP.NET padrão para ela
5. Reinicia o serviço IIS

O arquivo em lotes contém uma senha temporária codificada de "<strong>1pass\@Word</strong>", que será solicitado a inserir o comando runas quando o arquivo em lotes for executado. Após a conclusão do comando runas, a senha da conta ASPNET é recriada com um valor aleatório forte. Observe que o arquivo em lotes poderá falhar se a senha codificada não atender aos requisitos de complexidade de senha em seu ambiente. Se esse for o caso, você poderá alterá-lo para outro valor apropriado para o seu ambiente.

*> [!IMPORTANT]* Se você tiver adicionado configurações de controle de acesso personalizadas ou permissões de conta de banco de dados para a conta ASPNET, elas deverão ser recriadas após a conclusão desse arquivo em lotes. Isso ocorre porque, quando a conta é recriada, ela receberá um novo SID (identificador de segurança).

*> [!IMPORTANT]* Se você estiver executando o processo de trabalho do ASP.NET com uma conta personalizada diferente da conta ASPNET, não deverá executar esse arquivo em lotes. Em vez disso, você deve fazer logon interativamente ou usar o comando runas com essa conta, que criará um perfil de usuário para essa conta.

O arquivo em lotes está incluído no arquivo de extração automática abaixo. Para usá-lo:

1. Você deve estar executando como uma conta com privilégios de administrador
2. [Baixar e abrir o arquivo executável de extração automática](ms03-32-issue/_static/fixup1.exe)
3. Extrair o conteúdo para c:\
4. Selecione executar... no menu iniciar e digite `cmd.exe`
5. Nas janelas de comando abrir, digite `c:\fixup.cmd`.
6. Quando solicitado, insira <strong>1pass\@palavra</strong> como a senha.
7. Se você tiver personalizado anteriormente configurações de controle de acesso ou permissões de conta de banco de dados para a conta ASPNET, precisará aplicar novamente essas configurações agora.

Muitas desculpas para a inconveniência que isso causou. Postaremos informações adicionais conforme ele se tornar disponível.

A matriz abaixo detalha plataformas e versões afetadas por esse problema.

| .NET Framework | Plataforma | Afeta |
| --- | --- | --- |
| Versão 1,0 | Windows 2000 Professional | Não |
| Versão 1,0 | Windows 2000 Server | Não |
| Versão 1,0 | Windows XP Professional | Sim |
| Versão 1,0 | Windows Server 2003 | Não |
| Versão 1,0 | Windows XP Home com Cassini | Não |
| Versão 1.1 | Windows 2000 Professional | Não |
| Versão 1.1 | Windows 2000 Server | Não |
| Versão 1.1 | Windows XP Professional | Não |
| Versão 1.1 | Windows Server 2003 | Não |
| Versão 1.1 | Windows XP Home com Cassini | Não |

Atenciosamente,   
 A equipe do ASP.NET
