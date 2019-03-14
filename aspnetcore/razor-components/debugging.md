---
title: Depurar componentes de Razor
author: guardrex
description: Saiba como depurar aplicativos Blazor e componentes do Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/debug
ms.openlocfilehash: fb7ddcf3ae40ec28a372adf724a293b375be28a1
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57034113"
---
# <a name="debug-razor-components"></a>Depurar componentes de Razor

[Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

Componentes do Razor tem alguns *muito antecipada* suporte para depuração de aplicativos do lado do cliente Blazor em execução no WebAssembly no Chrome. Embora esse suporte de depuração inicial é muito limitado e unpolished, ele mostra a infra-estrutura básica de depuração andam juntos.

Para depurar um aplicativo de Blazor do lado do cliente no Chrome:

* Crie um aplicativo Blazor em `Debug` configuração (o padrão para aplicativos não publicados).
* Execute o aplicativo de Blazor no Chrome (versão 70 ou superior).
* Com o foco do teclado no aplicativo (não no painel de ferramentas de desenvolvimento, que você provavelmente deve fechar uma experiência de depuração menos confusa), selecione o atalho de teclado específicos Blazor seguinte:
  * `Shift+Alt+D` no Windows/Linux
  * `Shift+Cmd+D` no macOS

Execute o Chrome com a depuração remota habilitada para depurar o aplicativo Blazor. Se a depuração remota está desabilitada, uma página de erro é gerada pelo Chrome. A página de erro contém instruções para executar o Chrome com a porta de depuração aberta para que o proxy de depuração Blazor pode se conectar ao aplicativo. *Feche todas as instâncias do Chrome* e reinicie o Chrome conforme instruído.

![Página de erro de depuração Blazor](https://user-images.githubusercontent.com/1874516/43123091-01ec0796-8ed8-11e8-844c-23b4e6e9d069.png)

Depois que o Chrome estiver em execução com a depuração remota habilitada, o atalho de teclado depuração abre uma nova guia do depurador. Após alguns instantes, o *fontes* guia mostra uma lista dos assemblies do .NET no aplicativo. Expanda cada assembly e localize o *. CS*/*. cshtml* disponível para depuração de arquivos de origem. Defina pontos de interrupção, retorne para o guia do aplicativo e os pontos de interrupção são atingidos. Depois de um ponto de interrupção é atingido, passo a passo (`F10`) ou retomar (`F8`) normalmente.

![Depuração de Blazor](https://user-images.githubusercontent.com/1874516/43123060-efb0b3b0-8ed7-11e8-9ea5-97aa34247a0b.png)

Blazor fornece um proxy de depuração que implementa o [protocolo do Chrome DevTools](https://chromedevtools.github.io/devtools-protocol/) e aumenta o protocolo de com. Informações específicas do NET. Quando a depuração de atalho de teclado é pressionado, Blazor aponta o Chrome DevTools no proxy. O proxy se conecta à janela do navegador que você está procurando para depurar (portanto, a necessidade de habilitar a depuração remota).

Você deve estar se perguntando por que não apenas usamos os mapas de código-fonte do navegador. Mapas de origem permitir que o navegador mapear arquivos compilados para seus arquivos de origem original. No entanto, não é mapeado Blazor C# diretamente para JS/WASM (pelo menos não ainda). Em vez disso, Blazor faz interpretação IL dentro do navegador, de modo que os mapas de origem não são relevantes.

Observe que são os recursos do depurador **muito limitado**. Só é possível no momento:

* Definir e remover pontos de interrupção.
* Passo a passo por meio do código ou retomada (`F8`).
* No *Locals* exibir, observe os valores de todas as variáveis locais do tipo `int`, `string`, e `bool`.
* Ver a pilha de chamadas, incluindo cadeias de chamadas que vão do JavaScript em .NET e do .NET para JavaScript.

Você *não é possível*:

* Observe os valores de qualquer local que não é um `int`, `string`, ou `bool`.
* Observe os valores de quaisquer campos ou propriedades de classe.
* Passe o mouse sobre as variáveis para ver seus valores
* Avalie expressões no console.
* Etapa entre as chamadas assíncronas.
* Execute a maioria dos outros cenários comuns de depuração.

Desenvolvimento de cenários de depuração adicional é um foco em andamento da equipe de engenharia.

## <a name="troubleshooting-tip"></a>Dica de solução de problemas

Se você estiver executando em erros, a seguinte dica pode ajudar:

No **depurador** guia, abra as ferramentas de desenvolvedor no seu navegador. No console, execute `localStorage.clear()` para remover quaisquer pontos de interrupção.
