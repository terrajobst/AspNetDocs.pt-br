---
ms.openlocfilehash: 24c36493284988aa45b15c78e2577c0ef96aba56
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57029983"
---
<a name="jquery-validation-pluginhttpsjqueryvalidationorg---form-validation-made-easy"></a>[Plug-in do jQuery Validation](https://jqueryvalidation.org/) – validação de formulário mais fácil
================================

[![Status do Build](https://secure.travis-ci.org/jquery-validation/jquery-validation.svg)](https://travis-ci.org/jquery-validation/jquery-validation)
[![Status de devDependency](https://david-dm.org/jquery-validation/jquery-validation/dev-status.svg?theme=shields.io)](https://david-dm.org/jquery-validation/jquery-validation#info=devDependencies)

O Plug-in de Validação do jQuery oferece uma validação para seus formulários existentes, ao mesmo tempo que faz todos os tipos de personalização para se adequar ao seu aplicativo com facilidade.

## <a name="getting-started"></a>Guia de Introdução

### <a name="downloading-the-prebuilt-files"></a>Baixar os arquivos pré-criados

Os arquivos predefinidos podem ser baixados de https://jqueryvalidation.org/

### <a name="downloading-the-latest-changes"></a>Baixar as alterações mais recentes

Os arquivos de implantação não lançados podem ser obtidos por:

 1. [Baixar](https://github.com/jquery-validation/jquery-validation/archive/master.zip) ou criar bifurcação deste repositório
 2. [Configurar o build](CONTRIBUTING.md#build-setup)
 3. Execute `grunt` para criar os arquivos criados no diretório "dist"

### <a name="including-it-on-your-page"></a>Incluí-lo na sua página

Inclua o jQuery e o plug-in em uma página. Em seguida, selecione um formulário para validar e chame o método `validate`.

```html
<form>
    <input required>
</form>
<script src="jquery.js"></script>
<script src="jquery.validate.js"></script>
<script>
$("form").validate();
</script>
```

Como alternativa, inclua o jQuery e o plug-in via requirejs em seu módulo.

```js
define(["jquery", "jquery.validate"], function( $ ) {
    $("form").validate();
});
```

Para obter mais informações sobre como configurar regras e personalizações, [confira a documentação](https://jqueryvalidation.org/documentation/).

## <a name="reporting-issues-and-contributing-code"></a>Relatar problemas e código de contribuição

Confira as [Diretrizes de Contribuição](CONTRIBUTING.md) para obter detalhes.

**OBSERVAÇÃO IMPORTANTE SOBRE A VALIDAÇÃO DE EMAIL**. A partir da versão 1.12.0 este plug-in está usando a mesma expressão regular que a [especificação HTML5 sugere que os navegadores usem](https://html.spec.whatwg.org/multipage/forms.html#valid-e-mail-address). Vamos acompanhar e usar a mesma verificação. Se você acredita que as especificações estejam erradas, denuncie o erro. Se você tiver requisitos diferentes, considere [usar um método personalizado](https://jqueryvalidation.org/jQuery.validator.addMethod/).
Caso você precise ajustar os padrões de expressão regular da validação interna, [siga a documentação](https://jqueryvalidation.org/jQuery.validator.methods/).

**NOTA IMPORTANTE SOBRE O MÉTODO REQUERIDO**. A partir da versão 1.14.0, esse plug-in interrompe a remoção de espaços em branco do valor do elemento anexado. Se você deseja obter o mesmo resultado, pode usar o [`normalizer`](https://jqueryvalidation.org/normalizer/) que pode ser usado para transformar o valor de um elemento antes da validação. Este recurso estava disponível desde `v1.15.0`. Em outras palavras, você pode fazer algo parecido com isto:
``` js
$("#myForm").validate({
    rules: {
        username: {
            required: true,
            // Using the normalizer to trim the value of the element
            // before validating it.
            //
            // The value of `this` inside the `normalizer` is the corresponding
            // DOMElement. In this example, `this` references the `username` element.
            normalizer: function(value) {
                return $.trim(value);
            }
        }
    }
});
```

## <a name="license"></a>Licença
Direitos autorais &copy; Jörn Zaefferer<br>
Licenciado sob a licença MIT.
