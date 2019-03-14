---
ms.openlocfilehash: 3b33bb9bcab1aa7e5438c6fe3f2d7ba0833e7756
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57054723"
---
--

## <a name="reporting-an-issue"></a>Relatar um Problemas

1. Verifique se o problema que você está tratando pode ser reproduzido.
2. Use http://jsbin.com ou http://jsfiddle.net para fornecer uma página de teste.
3. Indique em quais navegadores o problema pode ser reproduzido. **Observação: Problemas no modo de compatibilidade do IE não serão resolvidos. Certifique-se de testar em um navegador real!**
4. Em qual versão do plug-in o problema pode ser reproduzido. Ele pode ser reproduzido depois de atualizar para a versão mais recente.

Os problemas de documentação também são rastreados no controlador de problemas de [Validação do jQuery](https://github.com/jzaefferer/jquery-validation/issues).
Contudo, as Solicitações Pull para melhorar os documentos são bem-vindas no repositório de [documentos de Validação jQuery](https://github.com/jzaefferer/validation-content).

**OBSERVAÇÃO IMPORTANTE SOBRE A VALIDAÇÃO DE EMAIL**. A partir da versão 1.12.0 este plug-in está usando a mesma expressão regular que a [especificação HTML5 sugere que os navegadores usem](https://html.spec.whatwg.org/multipage/forms.html#valid-e-mail-address). Vamos acompanhar e usar a mesma verificação. Se você acredita que as especificações estejam erradas, denuncie o erro. Se você tiver requisitos diferentes, considere [usar um método personalizado](http://jqueryvalidation.org/jQuery.validator.addMethod/).

## <a name="contributing-code"></a>Código de contribuição

Obrigado por contribuir! Estas são algumas diretrizes para ajudar a sua contribuição a ser implantada.

1. Verifique se o problema que você está tratando pode ser reproduzido. Use jsbin.com ou jsfiddle.net para fornecer uma página de teste.
2. Siga o [guia de estilo jQuery](http://contribute.jquery.com/style-guides/js)
3. Adicione ou atualize testes de unidade junto com a correção. Execute os testes de unidade em pelo menos um navegador (veja abaixo).
4. Execute `grunt` (veja abaixo) para verificar linting e outros problemas.
5. Descreva a alteração em sua mensagem de confirmação e faça referência ao tíquete, assim: "Demonstrações: Correção de bug delegado para demonstração de dinâmicas totais. Correções #51". Se você estiver adicionando um novo arquivo de localização, use algo parecido com isto: "Localização: Localização adicionado Croata (HR)"

## <a name="build-setup"></a>Configuração do build

1. Instalar [NodeJS](http://nodejs.org).
2. Instale a CLI do Assistente para instalação executando `npm install -g grunt-cli`. Mais detalhes estão disponíveis no site http://gruntjs.com/getting-started.
3. Instale as dependências de NPM executando `npm install`.
4. O build agora pode ser chamado executando `grunt`.

## <a name="creating-a-new-additional-method"></a>Criar um novo método adicional

Se você escreveu métodos personalizados que deseja contribuir para additional-methods.js:

1. Criar uma ramificação
2. Adicionar o método como um novo arquivo no `src/additional`
3. (Opcional) Adicionar traduções a `src/localization`
4. Envie uma solicitação pull para a ramificação mestre.

## <a name="unit-tests"></a>Testes de Unidade

Para executar testes de unidade, basta abrir `test/index.html` no seu navegador. Certifique-se de executar o `npm install` antes para que todas as dependências necessárias estejam disponíveis.
Inicie com um navegador enquanto desenvolve a correção e, em seguida, execute em todos os outros antes de confirmar. Geralmente, o Chrome, Firefox, Safari e Opera mais recentes e alguns IEs.

## <a name="documentation"></a>Documentação

Relate problemas de documentação no controlador de problemas de [Validação jQuery](https://github.com/jzaefferer/jquery-validation/issues).
Caso sua solicitação pull implemente ou altere a API pública, seria um extra fornecer uma solicitação pull em relação ao repositório de [Documentos de validação jQuery](https://github.com/jzaefferer/validation-content).

## <a name="linting"></a>Linting

Para executar o JSHint e outras ferramentas, use `grunt`.
